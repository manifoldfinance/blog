---
layout: post
current: post
cover:  assets/images/eth.png
navigation: True
title:  The Indifference Engine, part one
date: 2021-01-25 19:00:00
tags: [Applied Engineeering]
class: post-template
subclass: 'post'
author: sambacha
---
# The Indifference Engine: Building Functional Transactional Tracing at Scale

- [The Indifference Engine: Building Functional Transactional Tracing at Scale](#the-indifference-engine-building-functional-transactional-tracing-at-scale)
  - [Overview](#overview)
  - [Smart Contract Disclaimer & DevOps](#smart-contract-disclaimer--devops)
      - [Fail Safe Mechanism](#fail-safe-mechanism)
      - [Monitoring](#monitoring)
        - [Active Monitoring](#active-monitoring)
    - [The Problem with Logging today](#the-problem-with-logging-today)
    - [Why the focus?](#why-the-focus)
      - [Contravriant Functors](#contravriant-functors)
    - [A Modest Proposal for logging](#a-modest-proposal-for-logging)
      - [Aggregating Tracers](#aggregating-tracers)
    - [Tracers](#tracers)


## Overview 

This article is broken into two distinct sections:

1. Operational Procedures and defining system expectations 

2. Application of cutting edge engineering techniques in eliminating issues taken at face value typically. 

The information in this article is sourced from IOHK (Cardano) and Well Typed. 

IOHK's application of these theories can be found [in their devops framework, available on github.](https://github.com/input-output-hk/iohk-monitoring-framework)


## Smart Contract Disclaimer & DevOps

#### Fail Safe Mechanism

Smart contracts may not include appropriate or sufficient backup/failover mechanisms in case something goes awry.

Smart contracts may depend on other systems to fulfill contract terms. These other systems may have vulnerabilities that could prevent the smart contract from functioning as intended.

Some smart contract platforms may be missing critical system safeguards and customer protections.

Where smart contracts are linked to a blockchain, forks in the chain could create operational problems.

In case of an operational failure, recourse may be limited or non-existent — complete loss of a virtual asset is possible. 


#### Monitoring 

** We monitor **

* Governance. 
Smart contracts may require attention, action, and possible revision subject to appropriate governance and liability mechanisms.

* Application State
Low Level Monitoring of Deployed Contracts

* Suspicious Activity
Failsafe mechanism to return to us in case of attack or error

* Application Performance 
  - on-chain throughput (e.g. gas griefing attacks? sandwiching?, etc.)
  - frontend services (e.g. web application interface)
  - backend services (e.g. data endpoints for 3rd parties, etc)


##### Active Monitoring

* Smart Contract Monitoring:

Actively monitor one (or more) Ethereum smart contracts and user accounts (or any combination) watching for odd or “known-dangerous” transactional patterns. Report to anomalies to a list of email, SMS, web site, or individuals whenever something of interest happens.
End Users:  Smart contract developers, smart contract participants (i.e. token holders)
Notes:  “Weird” things include recursive attacks, violations of invariants (token balances to ether balance), largest purchases, most active trader accounts, etc.; Could potentially spawn an “insured” smart contract industry expectation.


* Smart Contract Reporting:


“Quarterly” reports may be available. On-demand reports generated for margin accounts, activity (report on token holders), individual ether holdings and transaction histories (i.e. bank statements) on a per-account, per-contract-group, by industry, or system wide.
End Users:  Smart contract developers, smart contract participants (i.e. token holders), economists, regulators. 


*NOTE*: Allows for self-reporting on business processes, expenditures, and revenue from outside an organization—​no need to wait for company reports; marketing efforts might engender an expectation that every smart contract’s accounting is fully transparent.
Auditing Support:

Provide data and transactional information to third parties not associated with the development team of a smart contract system. Interesting to potential investors, industry analysts, auditors and/or regulators.

End Users:  regulators, auditors, potential investors
*NOTE*: Fully parsed data makes for much easier auditing of smart contracts, could expose non-delivery of promised behavior.


### The Problem with Logging today 

- Too Much Overhead, 
- Too much decided 'away' from the site
-  plainly ment for ''blamestorming'


A new approach based on contravariant functors, that avoids cluttering the code, has a simple general interface that minimizes concrete dependencies and still allows a choice of logging backend.
>  Duncan Coutts - Contravariant Logging: How to add logging without getting grumpy


### Why the focus?

- Operational complexity reduced 
- Operational overhead reduced 
- Massive reduction in computing resources 
- Faster response time to incidents 

- It stands to reason at least an order of magnitude difference in capability. 


#### Contravriant Functors 

- Ways of achieving this capability in existing code with existing logging service (we use greylog2)
  
> Augment via Closure

> Augment via Results 

### A Modest Proposal for logging 

Trace typed values only

- no log names
- no severity
- no string messages

A mechanism for transforming traced values
- allows adding context
- convert from application types to logging types
  
Minimalist
- just a (tiny) interface
- no extra dependencies in application code
- can be instantiated with any logging backend


> Example 
> 
From Data.Functor.Contravariant in base
class Contravariant $(f: * \rightarrow *)$ where contramap $::(a \rightarrow b) \rightarrow f b \rightarrow f a$
Like fmap on Functor but the "other way around"
class Functor $(f:: * \rightarrow *)$ where fmap $::(a \rightarrow b) \rightarrow f a \rightarrow f b$
Easy way to remember:
- fmap transforms the output
- contramap transforms the input

> Another example

```haskell
sequence [addNewFetchRequest (contramap (TraceLabelPeer peer) clientStateTracer) blockFetchSize request gsvs stateVars (Right request, peer) $\leftarrow$ decisions $]$
```

The per-peer action does not need to know which peer id.
For tracing we want to know which peer it concerns, so we use
contramap (TraceLabelPeer peer) clientStateTracer
starting from outer tracer of type Tracer m (TraceLabelPeer peer (TraceFetchClientState header))
giving us the inner tracer of type
Tracer m (TraceFetchClientState header)


#### Aggregating Tracers

With many subsystems, we have many typed tracers
Aggregate using records

- - | All tracers of a node bundled together data Tracers remotePeer localPeer blk $m =$ Tracers \{chainSyncClientTracer
:: Tracer $m$ (TraceChainSyncClientEvent chainSyncServerHeaderTracer :: Tracer m (TraceChainSyncServerEvent chainSyncServerBlockTracer $\quad::$ Tracer $m$ (TraceChainSyncServerEvent
mempoolTracer
$\because$ Tracer $m$ (TraceEventMempool blk) forgeTracer
$\because$ Tracer $m$ (TraceForgeEvent blk) \}

### Tracers 

This style makes it relatively easy (and efficient) to turn tracers on/off

Pick your favorite logging backend
Make a top level Tracer to emit items to the logging backend
Tracer IO (Katip.Item a)

> Katip is a structured logging framework for Haskell.

Define functions to transform the traced values into log items
- near the component top level so all context is known
- select/filter what is needed
- can turn tracers on/off
- can depend on verbosity

