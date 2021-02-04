# Fact-orientation

People communicate facts, that is the fact is the unit of communication. The fact-oriented approach enables multidimensional categorization.[7]

The fact-oriented approach supports time changeability.
The fact-oriented approach provides semantic stability.
The fact-oriented approach enables extensibility and reuse.
The fact-oriented approach involves breaking down compound fact types into elementary (atomic) ones.
Conceptual formalization describes a business domain, and is composed of 1) a conceptual schema (fact structure) and 2) a population of ground facts. A business domain (universe of discourse) comprises those aspects of the business that are of interest.

The schema declares:

the relevant fact types (kinds of ground fact, e.g. Employee works for Department)
the relevant business rules (typically constraints or derivation rules).
A fact is a proposition taken to be true by the business. Population facts are restricted to elementary and existential facts.

### Constraints can be static or dynamic:

A static constraint imposes a restriction on what fact populations are possible or permitted, for each fact population taken individually e.g. Each Employee was born on at most one Date.
A dynamic constraint imposes a restriction on transitions between fact populations
e.g. a person’s marital status may change from single to married, but not from divorced to single

### Derivation of facts.

Derivation means either, how a fact type may be derived from one or more other fact types e.g.
Person1 is an uncle of Person2 if Person1 is a brother of some Person3 who is a parent of Person2
Or, how a noun concept (object type) may be defined in terms of other object types and fact types e.g.
Each FemaleAustralian is a Person who was born in Country ‘Australia’ and has Gender ‘Female’

# Rule-based approach

Rules play a very important role in defining business semantics: they can influence or guide behaviours and support policies, responding to environmental situations and events. This means that rules represent the primary means by which an organization can direct its business, defining the operative way to reach its objectives and perform its actions.

The rule-based approach aims to address two different kinds of users:

it addresses business communities, in order to provide them with a structured approach, based on a clear set of concepts and used to access and manage business rules;
it addresses IT professionals, in order to provide them with a deep understanding about business rules and to help them in models creation. The rules-based approach also helps bridge the rift that can occur between the data managers and the software designers.
The essence of the rule-based conceptual formalizations is that rules build on facts, and facts build on concepts as expressed by terms.[8]

This mantra is memorable, but a simplification since in SBVR: Meaning is separate from expression; Fact Types (Verb Concepts) are built on Noun Concepts; Noun Concepts are represented by Terms; and Fact Types are represented by Fact Symbols (verb phrases).

Rule statements are expressed using either alethic modality or deontic modality and require elements of modal logic as formalization.

SBVR Structural Business Rules use two alethic modal operators:

it is necessary that …
it is possible that …
SBVR Operative Business Rules use two deontic modal operators:

it is obligatory that …
it is permitted that …
Structural business rules (static constraints) are treated as alethic necessities by default, where each state of the fact model corresponds to a possible world. Pragmatically, the rule is understood to apply to all future states of the fact model, until the rule is revoked or changed. For the model theory, the necessity operator is omitted from the formula. Instead, the rule is merely tagged as a necessity. For compliance with Common Logic, such formulae can be treated as irregular expressions, with the necessity modal operator treated as an uninterpreted symbol.

If the rule includes exactly one deontic operator, e.g. O (obligation), and this is at the front, then the rule may be formalized as Op, where p is a first-order formula that is tagged as obligatory. In SBVR, this tag is assigned the informal semantics: it ought to be the case that p (for all future states of the fact model, until the constraint is revoked or changed). From a model-theoretic perspective, a model is an interpretation where each non-deontic formula evaluates to true, and the model is classified as: a permitted model if the p in each deontic formula (of the form Op) evaluates to true, otherwise the model is a forbidden model (though still a model). This approach removes any need to assign a truth value to expressions of the form Op.
