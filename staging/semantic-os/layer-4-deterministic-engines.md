# Layer 4: Deterministic Engines

**MLIR compilation and reproducible execution**

Layer 4 provides deterministic compilation and execution infrastructure. This layer takes Layer 1's semantic IR and lowers it to efficient, reproducible executable code using MLIR (Multi-Level Intermediate Representation) and other compilation targets.

---

## What This Layer Does

**Purpose:** Compile semantic programs to deterministic, optimized, executable code.

**Key Responsibilities:**
- Lower semantic IR to MLIR
- Apply domain-specific optimizations
- Generate executable code (native, WebAssembly, GPU)
- Ensure bitwise-identical reproducibility
- Provide runtime execution environment
- Support multiple compilation targets

**Analogy:** Like the LLVM backend for programming languages—takes IR and generates optimized machine code.

---

## Projects at This Layer

### ✅ Production Projects

#### [Morphogen](https://github.com/semantic-infrastructure-lab/morphogen)
**Status:** Production v0.11 | **MLIR Backend:** Complete

**What it does:** Compiles cross-domain programs (audio, physics, circuits) to MLIR and then to optimized execution targets.

**Key innovations:**
- MLIR-based compilation pipeline
- Multirate scheduling (different clocks in same program)
- Deterministic execution (bitwise reproducibility)
- WebAudio and GPU backends
- Cross-domain optimization passes

**Compilation flow:**
```
Morphogen DSL (Layer 2)
    ↓ Parse + Type Check
Pantheon IR (Layer 1)
    ↓ Lower to MLIR
MLIR (Layer 4)
    ↓ Optimize + Generate Code
WebAudio / Native / GPU
```

**Determinism guarantee:** Same inputs + same code = bitwise-identical outputs across platforms.

**Use cases:**
- Audio synthesis that sounds identical everywhere
- Physics simulations with reproducible results
- Cross-platform deterministic computation
- Regression testing for generated output

---

#### [RiffStack](https://github.com/semantic-infrastructure-lab/riffstack)
**Status:** Production v0.8 | **Audio Engine:** Complete

**What it does:** REPL-driven audio synthesis with live-coding capabilities and deterministic playback.

**Execution model:**
- Compiles expressions to executable audio graphs
- Real-time parameter updates
- Deterministic rendering
- Cross-session reproducibility

**Pattern:**
```
User types expression in REPL
    ↓ Compile to audio graph
    ↓ Execute in real-time
Hear results immediately
    ↓ Tweak parameters
    ↓ Recompile incrementally
Iterate at speed of thought
```

**Technical highlights:**
- Sub-millisecond compilation
- Zero-copy audio buffers
- Incremental recompilation
- Session replay (exact same audio output)

---

## Why This Layer Matters

**Without Layer 4:**
- Semantic programs have no execution
- Non-deterministic results (hard to debug)
- Poor performance
- Platform-specific behavior

**With Layer 4:**
- Semantic programs run efficiently
- Bitwise reproducibility
- Optimized execution
- Cross-platform consistency

---

## Design Patterns

### The Determinism Contract

**Guarantee:** Given identical inputs and code, outputs are bitwise-identical across:
- Different machines
- Different operating systems
- Different runs on same machine
- Different times (today vs. 6 months from now)

**Why it matters:**
- Reproducible research
- Reliable regression testing
- Auditable AI outputs
- Cross-platform consistency

**How we achieve it:**
- Deterministic compilation
- Fixed-point arithmetic where needed
- Controlled floating-point operations
- Explicit random seeds
- No implicit state

### MLIR Compilation Pipeline

```
Semantic IR (Layer 1)
    ↓ Dialectize
MLIR Custom Dialects
    ↓ Lower to Standard Dialects
MLIR Standard (func, arith, scf)
    ↓ Optimize
MLIR Optimized
    ↓ Lower to Target
LLVM IR / WebAssembly / GPU code
    ↓ Codegen
Executable
```

### Multirate Scheduling

Different parts of program run at different rates:

```yaml
audio:
  rate: 48000 Hz    # Audio samples
  buffer: 512       # Process in chunks

physics:
  rate: 240 Hz      # Physics steps
  substeps: 4       # Subdivide if needed

ui:
  rate: 60 Hz       # Frame rate
```

Scheduler ensures:
- Correct temporal relationships
- No data races
- Deterministic interleaving
- Efficient execution

### Runtime Guarantees

- **Memory safety:** No buffer overflows
- **Type safety:** Runtime types match compile-time types
- **Temporal safety:** Events happen in correct order
- **Determinism:** Same inputs → same outputs

---

## Real-World Examples

### Morphogen Audio Synthesis

```yaml
# User writes this
oscillator:
  frequency: 440 Hz
  waveform: sine

# Compiles to MLIR
func @oscillator(%time: f64) -> f64 {
  %freq = arith.constant 440.0 : f64
  %two_pi = arith.constant 6.283185307179586 : f64
  %phase = arith.mulf %time, %freq : f64
  %angle = arith.mulf %phase, %two_pi : f64
  %result = math.sin %angle : f64
  return %result : f64
}

# Executes deterministically
# Same 440 Hz sine wave every single time
```

### RiffStack Live Coding

```
> (sin 440)
# Compiles + plays immediately
# Hear a 440 Hz sine tone

> (sin (* 440 1.5))
# Recompiles + updates in real-time
# Hear a 660 Hz sine tone (perfect fifth)

# Save session, replay later
# Exact same audio output
```

---

## Technical Details

### Optimization Passes

- **Dead code elimination:** Remove unused computations
- **Constant folding:** Compute constants at compile time
- **Loop unrolling:** Optimize tight loops
- **Vectorization:** SIMD operations
- **Inlining:** Reduce function call overhead

### Execution Targets

- **Native code:** Via LLVM backend
- **WebAssembly:** Browser execution
- **WebAudio:** Browser audio API
- **GPU:** CUDA/Metal/Vulkan (planned)

### Performance Characteristics

- **Compilation:** Sub-second for typical programs
- **Execution:** Near-native performance
- **Memory:** Zero-copy where possible
- **Latency:** Sub-millisecond for audio

---

## Related Layers

- **Layer 1 (Universal IR):** Source of semantic programs
- **Layer 2 (Domain Modules):** Domain languages compile through Layer 1 to Layer 4
- **Layer 3 (Agent Orchestration):** Agents may trigger compilation/execution
- **Layer 5 (Human Interfaces):** Users interact with executing programs

---

## Technical Status

**Morphogen (Production):**
- ✅ MLIR compilation pipeline
- ✅ Multirate scheduling
- ✅ WebAudio backend
- ✅ Deterministic execution
- ✅ Cross-domain optimization
- 🔬 GPU backend (research)

**RiffStack (Production):**
- ✅ REPL execution
- ✅ Real-time compilation
- ✅ Session replay
- ✅ Incremental updates

**Next Steps:**
- GPU compilation targets
- More aggressive optimizations
- Better error messages from MLIR
- Profiling and debugging tools
- Cross-domain fusion passes

**Learn More:**
- [Morphogen Repository](https://github.com/semantic-infrastructure-lab/morphogen)
- [RiffStack Repository](https://github.com/semantic-infrastructure-lab/riffstack)
- [Layer 1: Universal IR](layer-1-universal-ir.md) for compilation source
- [Unified Architecture Guide](../architecture/UNIFIED_ARCHITECTURE_GUIDE.md)
