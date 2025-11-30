# **SIL Principles (v1)**

*Durable constraints for building semantic infrastructure.*

---

## **0. Purpose of the Principles**

These principles define how SIL conducts research, designs systems, and evaluates correctness.
They are constraints, not values.
They exist to keep the Semantic OS coherent, inspectable, reproducible, and extensible over long time horizons.

They apply to every layer, every domain, every operator, and every contribution.

### **Relationship to DESIGN_PRINCIPLES**

These 14 principles govern the **research infrastructure and Semantic OS architecture**. They are foundational constraints for the entire system.

For **practical engineering guidance** when implementing SIL projects (Pantheon, Morphogen, TiaCAD, etc.), see [DESIGN_PRINCIPLES.md](../architecture/DESIGN_PRINCIPLES.md), which distills engineering methodology into 5 operational principles:

1. **Clarity** - See the system
2. **Simplicity** - Model the system
3. **Composability** - Extend the system
4. **Correctness** - Keep the system safe
5. **Verifiability** - Trust the system

Both sets of principles are complementary: these 14 constrain the *what* (the infrastructure), while the 5 guide the *how* (the implementation).

---

## **1. Principles**

### **1. Structure Before Heuristics**

All SIL systems prioritize explicit structure—schemas, types, relations, operators—before heuristics or statistical inference.
Heuristics may propose; structure decides.

### **2. Meaning Must Be Explicit**

All meaningful objects must be represented as typed, inspectable semantic structures.
Implicit meaning is not permitted in core representations.

### **3. Provenance Everywhere**

Every transformation must produce a provenance record: inputs, outputs, operator, assumptions, and context.
No silent changes.

### **4. Invariants Define Correctness**

Correctness is defined by invariants, not by expectation or intuition.
All operators must either preserve declared invariants or fail explicitly.

### **5. Determinism When Promised, Bounded Reproducibility When Not**

When operations declare determinism, the system must enforce it.
When full determinism is infeasible, operators must define equivalence relations and tolerances, and produce metadata that makes variability inspectable.

### **6. Cross-Domain Coherence Is a First-Class Goal**

Domain schemas, operators, and invariants must fit into a unified semantic substrate.
No domain is allowed to form an isolated island.

### **7. Operators Are the Only Way to Change State**

All mutations of semantic objects, USIR graphs, and workflows must occur through declared operators.
No direct writes, no bypasses, no implicit edits.

### **8. Version Everything**

Schemas, operators, domains, objects, and mappings must be versioned.
Nothing substantial may change without recording what changed and why.

### **9. Visibility and Inspectability Are Mandatory**

Users and agents must be able to inspect structure, provenance, operator chains, and validation outcomes.
Opaque internals are not acceptable.

### **10. Reproducibility Over Performance**

Whenever there is a conflict between reproducibility and performance, reproducibility wins.
Performance can improve; lost traceability cannot.

### **11. Stability of Contracts Over Breadth of Features**

SIL favors stable, minimal interfaces over feature-rich but drifting APIs.
A small number of well-defined contracts outperforms a large number of ad hoc capabilities.

### **12. Play + Rigor as the Discovery Method**

Exploration, tinkering, hypothesis generation, and prototyping are encouraged—
but nothing enters the substrate without formalization, validation, and provenance.

### **13. Stewardship Protects Coherence**

Openness is encouraged, but stewards maintain the coherence of semantic memory, schemas, types, and operators.
All contributions enter through review for structural correctness.

### **14. Representations and Operators Are Long-Lived Artifacts**

The Semantic OS is infrastructure.
Schema and operator longevity matters more than short-term convenience or trends.

---

## **2. Boundary Notes (Clarifications)**

* These principles do **not** prohibit the use of ML models—only untraceable reasoning.
* They do **not** require perfect determinism—only clear declaration of limits.
* They do **not** demand universal formalization—only that formalized components obey the substrate.
* They do **not** enforce one epistemology—only that epistemic commitments are explicit and inspectable.

---

## **3. Change Policy**

These principles evolve only when:

1. A change clearly improves semantic clarity or system integrity;
2. The change is versioned, documented, and justified;
3. Integrity tests confirm compatibility;
4. Provenance captures the rationale for evolution.

Principles change slowly. Coherence changes never.