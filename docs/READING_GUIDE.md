# SIL Documentation - Reading Guide

**Welcome to the Semantic Infrastructure Lab documentation.**

This guide helps you navigate SIL's technical documentation efficiently. Choose your reading path based on your goal.

**Last Updated:** 2025-11-29

---

## 🧭 Quick Navigation

### **"I want a 5-minute overview"**
→ **`canonical/SIL_MANIFESTO.md`** - Why SIL exists, what we're building

### **"I want the complete technical architecture"**
→ **`architecture/UNIFIED_ARCHITECTURE_GUIDE.md`** ⭐ **THE ROSETTA STONE**

### **"I need to look up a term"**
→ **`canonical/SIL_GLOSSARY.md`** - Keep this open while reading

### **"I want the formal specification"**
→ **`canonical/SIL_TECHNICAL_CHARTER.md`** - Complete 6-layer spec

### **"I want design principles"**
→ **`architecture/DESIGN_PRINCIPLES.md`** - How to evaluate designs

### **"I want the complete vision"**
→ **`vision/SIL_VISION_COMPLETE.md`** - Long-term aspirations

### **"I want to see what's built"**
→ **`../projects/PROJECT_INDEX.md`** - 11 SIL projects mapped by layer

---

## 📖 Curated Reading Paths

### **Path 1: "Quick Start" (1 hour)**
**Perfect for:** New team members, potential collaborators

```
1. canonical/SIL_MANIFESTO.md (10 min)
   ↓ Understand why SIL exists

2. architecture/UNIFIED_ARCHITECTURE_GUIDE.md (30 min) ⭐
   ↓ Learn the universal pattern

3. architecture/DESIGN_PRINCIPLES.md (15 min)
   ↓ Understand how we build

4. ../projects/PROJECT_INDEX.md (5 min)
   ↓ See what exists
```

**Output:** You understand the vision, the pattern, the principles, and what's built.

---

### **Path 2: "Implementation" (2-3 hours)**
**Perfect for:** Engineers building on SIL

```
1. architecture/UNIFIED_ARCHITECTURE_GUIDE.md (30 min) ⭐
   ↓ Learn the pattern

2. canonical/SIL_GLOSSARY.md (15 min - keep open)
   ↓ Learn the vocabulary

3. architecture/DESIGN_PRINCIPLES.md (15 min)
   ↓ Learn how to evaluate designs

4. canonical/SIL_TECHNICAL_CHARTER.md (1-2 hours)
   ↓ Understand the formal contracts

5. ../projects/PROJECT_INDEX.md (10 min)
   ↓ See concrete implementations
```

**Output:** You can design and implement SIL-compliant systems.

---

### **Path 3: "Complete Mastery" (4-6 hours)**
**Perfect for:** Core team, stewards, deep contributors

```
1. canonical/SIL_MANIFESTO.md (10 min)
   ↓ Start with the "why"

2. architecture/UNIFIED_ARCHITECTURE_GUIDE.md (30 min) ⭐
   ↓ Learn the pattern

3. canonical/SIL_GLOSSARY.md (15 min - keep open)
   ↓ Vocabulary reference

4. canonical/SIL_PRINCIPLES.md (10 min)
   ↓ The 14 research infrastructure principles

5. architecture/DESIGN_PRINCIPLES.md (15 min)
   ↓ The 5 engineering principles

6. canonical/SIL_TECHNICAL_CHARTER.md (2 hours)
   ↓ The complete formal specification

7. canonical/SIL_RESEARCH_AGENDA_YEAR1.md (30 min)
   ↓ Research roadmap

8. vision/SIL_VISION_COMPLETE.md (15 min)
   ↓ Long-term aspirations

9. ../projects/PROJECT_INDEX.md (30 min)
   ↓ All 11 projects in depth
```

**Output:** Complete understanding of SIL's philosophy, architecture, and roadmap.

---

## 👤 By Role

### **New Engineer/Contributor**
Start with: **Path 1 (Quick Start)** → then **Path 2 (Implementation)**

### **Architect/Steward**
Go straight to: **Path 3 (Complete Mastery)**

### **Researcher/Academic**
Focus on: `canonical/SIL_RESEARCH_AGENDA_YEAR1.md` + `canonical/SIL_TECHNICAL_CHARTER.md`

### **Just Curious**
Start with: `canonical/SIL_MANIFESTO.md` (10 min)

---

## 📚 Document Categories

### **Canonical Founding Documents** (Start Here)
The definitive specifications and founding principles.

- **SIL_MANIFESTO.md** (12KB) - Why SIL exists
- **SIL_TECHNICAL_CHARTER.md** (29KB) - The formal 6-layer specification
- **SIL_GLOSSARY.md** (8KB) - Canonical vocabulary
- **SIL_PRINCIPLES.md** (5KB) - 14 research infrastructure principles
- **SIL_RESEARCH_AGENDA_YEAR1.md** (19KB) - Research roadmap

### **Architecture Guides** (The Pattern)
How to think architecturally about semantic systems.

- **UNIFIED_ARCHITECTURE_GUIDE.md** ⭐⭐⭐ - The Rosetta Stone (THE most important doc)
- **DESIGN_PRINCIPLES.md** - The 5 principles for evaluating designs

### **Vision & Philosophy**
Long-term aspirations and philosophical foundations.

- **SIL_VISION_COMPLETE.md** - What the world looks like if SIL succeeds

### **Project Catalog**
What has been built.

- **../projects/PROJECT_INDEX.md** - All 11 SIL projects mapped to 6-layer architecture

### **Guides & Meta**
- **FOUNDER_BACKGROUND.md** - Who built this and why

---

## 📊 Documentation Structure

```
docs/
├── READING_GUIDE.md        ← You are here
│
├── canonical/              # Founding documents (START HERE)
│   ├── SIL_MANIFESTO.md
│   ├── SIL_TECHNICAL_CHARTER.md
│   ├── SIL_GLOSSARY.md
│   ├── SIL_PRINCIPLES.md
│   ├── SIL_RESEARCH_AGENDA_YEAR1.md
│   └── FOUNDERS_LETTER.md
│
├── architecture/           # The pattern (CRITICAL)
│   ├── UNIFIED_ARCHITECTURE_GUIDE.md  ⭐⭐⭐
│   └── DESIGN_PRINCIPLES.md
│
├── vision/                 # Long-term vision
│   └── SIL_VISION_COMPLETE.md
│
├── guides/                 # How-to guides
│   └── OPTIMIZATION_IN_SIL.md
│
├── operations/             # Operational docs
│   └── DEPLOYMENT_STANDARDS.md
│
└── meta/                   # Meta-documentation
    ├── FOUNDER_BACKGROUND.md
    └── EXTRACTION_MANIFEST.md
```

---

## 🎯 Tips for Reading

### 1. **Keep the Glossary Open**
`canonical/SIL_GLOSSARY.md` defines every core term. Open it in a second window and reference it as you read.

### 2. **Start with UNIFIED_ARCHITECTURE_GUIDE**
If you can only read ONE document, make it `architecture/UNIFIED_ARCHITECTURE_GUIDE.md`. It's the Rosetta Stone that makes everything else click.

### 3. **Don't Skip the Manifesto**
`canonical/SIL_MANIFESTO.md` explains WHY we're building this. Understanding the problem is essential to understanding the solution.

### 4. **Read in Order**
The reading paths above are sequenced intentionally. Each document builds on the previous one.

### 5. **Take Your Time with the Charter**
`canonical/SIL_TECHNICAL_CHARTER.md` is dense (29KB, 2-4 hours). Don't rush it. It's the complete formal specification.

---

## 📖 Cross-References

### If you're reading this and want to know:
- **Where projects are located on disk:** See `/home/scottsen/src/tia/projects/SIL/SIL_ECOSYSTEM_PROJECT_LAYOUT.md`
- **How projects map to architecture layers:** See `../projects/PROJECT_INDEX.md`
- **Relationship between PRINCIPLES and DESIGN_PRINCIPLES:** Both docs now cross-reference each other

---

## ❓ Still Lost?

**Can't find what you need?**
1. Check `canonical/SIL_GLOSSARY.md` for term definitions
2. Search the docs directory: `grep -r "your search term" docs/`
3. See `../projects/PROJECT_INDEX.md` for project-specific docs

**Want to understand the ecosystem?**
- See `../projects/PROJECT_INDEX.md` for all 11 SIL projects

**Want to understand file locations?**
- See `/home/scottsen/src/tia/projects/SIL/SIL_ECOSYSTEM_PROJECT_LAYOUT.md` (TIA workspace)

---

## 📝 Reference Docs to Keep Open

While working with SIL:
1. **`canonical/SIL_GLOSSARY.md`** - Term definitions
2. **`architecture/DESIGN_PRINCIPLES.md`** - Evaluation criteria
3. **`architecture/UNIFIED_ARCHITECTURE_GUIDE.md`** - The pattern

---

**The most important document:** `architecture/UNIFIED_ARCHITECTURE_GUIDE.md` ⭐⭐⭐

Start there if you're impatient or confused.

---

**Last Updated:** 2025-11-29
**Canonical Location:** `/home/scottsen/src/projects/SIL/docs/READING_GUIDE.md`
