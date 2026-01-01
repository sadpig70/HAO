# TQP v3.5 Unified Upgrade Work Plan (Gantree)

**Version:** 2.0  
**Date:** 2025-12-27  
**Based on Review:** TQP_Integrated_Review_Paper.md + TQP_Integrated_Review_Technical.md (8 AI Integration)  
**Goal:** PRX Quantum Submission + Production-Ready

---

## Design Principles

1. **PRX Submission Priority** - Consider paper/technical work dependencies
2. **Top-Down BFS** approach design
3. Decomposition to **Atomic Nodes** (AI implementable within 15 minutes)
4. Separate root when decomposition exceeds **5 levels**

---

## Dependency Analysis

```text
[Paper Work]              [Technical Work]           [Dependency]
─────────────────────────────────────────────────────────────
Benchmark Fairness  ←──── E2E/Kernel Separation    (Tech→Paper) ✅ Done
Spinoza Comparison  ←──── tqp_vs_spinoza.rs impl   (Tech→Paper) ✅ Done
Additional Molecule ←──── BeH₂ IBM Execution       (Tech→Paper)
Data Availability   ←──── GitHub Dataset Cleanup   (Tech→Paper) ✅ Done
Reference Expansion ───── (Independent)            (Paper Only) ✅ Done
Sparse Memory       ───── (Independent)            (Tech Only)
GPU PoC             ───── (Independent)            (Tech Only)
```

---

## Unified Work Tree

```text
TQP_v3.5_Unified_Upgrade // v3.5 Unified Upgrade (InProgress)
    Phase_0_Immediate // Immediate Fixes - Before PRX Submission (1 week) (Done)
        P0_Paper_Abstract // Paper Abstract Revision (Done)
            Add_Python_Overhead_Caveat // Add "includes Python overhead" (Done)
            Add_Crossover_Mention // Mention N≥17 crossover (Done)
            Tone_Down_Speedup // "2-3000x" → "end-to-end" restatement (Done)
        P0_Tech_Benchmark_Caveat // Add Technical Spec Caveat (Done)
            Add_Caveat_Text // Add Section 6 benchmark note (Done)
            Fix_M_Scaling // Fix "Near-linear (R²=0.98)" expression (Done)
        P0_Tent_Definition // Define T̂_ent f(m) (Done)
            Verify_fm_Unitarity // Prove f(m) unitarity mathematically first (Done)
            Define_fm_XOR // f(m) = m XOR (1 << (m % n)) (Done)
            Add_N2M2_Example // Add N=2, M=2 example (Done)
            Prove_Unitarity // Include unitarity proof in paper (Done)
        P0_Hardware_Accuracy // Revise Hardware Accuracy Interpretation (Done)
            Get_FCI_Reference // Specify FCI source (NIST/Literature) (Done)
            Change_Ref_to_FCI // Change reference from HF to FCI (Done)
            Add_ChemAcc_Warning // Explicitly state >1.6mHa (Done)
        P0_Data_Availability // Data Availability Statement (Done)
            Write_DAS // Write Statement (Done)
            Prepare_GitHub_Raw // Prepare raw_data folder (Done)
        P0_Hint_Clarify // Clarify H_int Usage (Done)
            Check_Usage // Check H_int usage in code (Done)
            Move_Future_Work // Classify as "future work" if unused (Done)
        P0_Backend_Unify // Unify Backend (Done)
            Update_Appendix // Unify to ibm_torino (Done)
    Phase_1_PRX_Ready // PRX Submission Preparation (3 weeks) (InProgress)
        P1_References_Expand // Expand References (Done)
            Add_21_References // Expand from 3 to 21 (Done)
        P1_Fair_Benchmark // Design and Execute Fair Benchmark (Done)
            Design_Fair_Protocol // Fair Comparison Protocol (Done)
                Option_B_Warmup_Qiskit // Measure after 10 warm-up runs (Done)
            Separate_E2E_Kernel // Separate E2E vs Kernel Time (Done)
                Measure_Python_Overhead // Measure Python Overhead (Done)
            Run_Fair_Benchmark // Execute Benchmark (Done)
                Run_N14_to_N20 // N=14-20, 30 repetitions (Done)
            Update_Paper // Update Paper (Done)
                Add_Section_3_4_1 // Add Methods Python Overhead (Done)
                Add_Section_5_3_1 // Add Discussion Interpretation (Done)
        P1_Spinoza_Benchmark // Spinoza Rust-to-Rust Comparison [NEW] (Done)
            Install_Nightly // Install Rust nightly (Done)
            Add_Spinoza_Dep // Add Cargo.toml dependency (Done)
            Write_Benchmark // Write tqp_vs_spinoza.rs (Done)
            Run_Init_Benchmark // State initialization comparison (Done)
            Run_Gate_Benchmark // Gate operation comparison (Done)
            Update_Paper_342 // Add Section 3.4.2 (Done)
            Update_Limitations // Update Section 5.3 (Done)
        P1_H2_Statistics // H₂ Statistical Verification [NEW] (Done)
            Run_5_Trials // Run 5 repetitions (Done)
            Calculate_Mean // Mean -4.2 mHa (Done)
            Update_Abstract // Update Abstract value (Done)
            Update_Conclusion // Update Conclusion value (Done)
        P1_Crossover_Precision // Crossover Analysis Refinement (Done)
            Measure_N14_to_N20 // Measure N=14-20, 30 times each (Done)
            Report_Results // Save results to CSV (Done)
        P1_Time_Concept // Clarify "Time as Resource" (Design)
            Separate_Physics_DS // Separate physics model vs data structure (Design)
            Add_Photonic_Relation // Relate to time-bin photonic QC (Design)
        P1_Tensor_Network // Concretize Tensor Network Connection (Design)
            Define_MPS_Eq // TQP = MPS(χ≤2^M) equation (Design)
        P1_PRX_Submit // PRX Submission Final Preparation (InProgress)
            Sync_TEX // TQP_PRX.tex ← draft_v1.md sync (InProgress)
            Compile_PDF // Recompile PDF (InProgress)
            Prepare_SM // Prepare Supplementary Material (InProgress)
            Prepare_Rebuttal_Template // Reviewer response template (InProgress)
            Final_Review // Final Review (InProgress)
            Submit_Draft // Submit Draft (Design)
    Phase_2_Extension // Technical Extension - Post Submission (2 months) (InProgress)
        P2_External_Dependency // External Dependency Check (Done)
            Check_IBM_Quota // Check IBM quota (Done: Insufficient)
            Check_PySCF_Version // Check PySCF version compatibility (Done)
        P2_BeH2_Validation // BeH₂ 14-qubit Validation (Done: Simulation Only)
            Generate_BeH2_Hamiltonian // Generate 14-qubit 666-term (Done)
            Add_to_Paper // Add Section 4.4, SM S5 (Done)
            Run_IBM_Hardware // Run on IBM hardware (Hold: Quota Insufficient)
        P2_Sparse_Memory // Sparse Memory Implementation (Decomposed)
        P2_Error_Handling // Systematize Error Handling (Decomposed)
        P2_TQP_Optimization // TQP Performance Optimization [NEW] (Design)
            SoA_Memory_Refactor // Struct of Arrays memory layout (Design)
            SIMD_Integration // AVX-512 SIMD extension (Design)
            Temporal_Pipeline // Time-axis pipeline parallelization (Design)
    Phase_3_Commercialization // Commercialization Preparation (3 months) (Design)
        P3_Resource_Estimate // Resource Estimation (Design)
            Estimate_FTE // Estimated personnel (FTE) (Design)
            Estimate_Cost // Estimated cost (Design)
        P3_Milestone_Checkpoint // Bi-weekly checkpoints (Design)
        P3_GPU_PoC // GPU PoC Implementation (Decomposed)
            GPU_Requirements // CUDA CC 7.0+, VRAM 8GB+ (Design)
        P3_Package_Deploy // PyPI/Crates Deployment (Decomposed)
        P3_Multi_Backend // Multi-backend (Hold)
```

---

## Decomposed Subtrees

### P1_Spinoza_Benchmark (Done) [NEW]

```text
P1_Spinoza_Benchmark // Spinoza Rust-to-Rust Comparison (Done)
    Install_Nightly // Install Rust nightly 1.94.0 (Done)
    Add_Spinoza_Dep // Add spinoza dependency to Cargo.toml (Done)
    Write_Benchmark // Write tqp_vs_spinoza.rs (Done)
        Init_Benchmark // State initialization comparison code (Done)
        Gate_Benchmark // Hadamard, PauliX, sequence (Done)
    Run_Benchmark // Run cargo +nightly bench (Done)
    Analyze_Results // Analyze results (Done)
        Init_TQP_1_4_1_9x // State init TQP 1.4-1.9× advantage (Done)
        Gate_TQP_9_17x // Gate ops TQP 9-17× advantage (Done)
        Crossover_N14_16 // Crossover point N≈14-16 (Done)
    Update_Paper // Update draft_v1.md (Done)
        Add_Section_342 // Add §3.4.2 (Done)
        Update_Limitations // Update §5.3 warning (Done)
    Update_Spec // Write TQP_Integrated_Specification_3.5.md (Done)
```

### P2_BeH2_Validation (Decomposed)

```text
P2_BeH2_Validation // BeH₂ 14-qubit Hardware Validation (Design)
    Generate_BeH2_Hamiltonian // Generate Hamiltonian with PySCF (Done)
        Setup_PySCF // Setup PySCF environment (Done)
        Run_CASSCF // Run CASSCF(3,2) calculation (Done)
        Convert_JordanWigner // Jordan-Wigner transformation (Done)
    Run_IBM_Validation // Run on IBM hardware (Hold)
        Prepare_QASM // Generate QASM 3.0 (Design)
        Submit_Estimator // Submit Estimator V2 (Design)
        Collect_Results // Collect results (Design)
    Apply_Error_Mitigation // Apply error mitigation (Design)
        Apply_ZNE // Zero Noise Extrapolation (Design)
        Report_Mitigated_Error // Report mitigated error (Design)
    Update_Paper_Results // Update paper results (Design)
```

### P2_Sparse_Memory (Decomposed)

```text
P2_Sparse_Memory // Sparse Memory Implementation (8/8 agreement) (Design)
    Design_Sparse_Format // Select CSR/COO format (Design)
        Analyze_Sparsity_Pattern // Analyze TQP state sparsity (Design)
        Select_Format // Determine format based on pattern (Design)
    Implement_SparseState // SparseState struct (Design)
        Define_Struct // Define fields (Design)
        From_Dense // Dense→Sparse conversion (Design)
        To_Dense // Sparse→Dense conversion (Design)
    Implement_Sparse_Ops // Implement sparse operations (Design)
        Sparse_Gate // Sparse gate application (Design)
        Sparse_FastMux // Sparse FastMux shift (Design)
        Sparse_Measure // Sparse measurement (Design)
    Dynamic_Policy // Dynamic policy selector (Design)
        Calc_Memory // Calculate memory requirements (Design)
        Select_Auto // Auto-select Dense/Sparse (Design)
    Test_N30 // Test N=30 on 64GB (Design)
```

### P2_Error_Handling (Decomposed)

```text
P2_Error_Handling // Systematize Error Handling (6/8 agreement) (Design)
    Define_TqpError // Define error types (Design)
        Create_Enum // Create TqpError enum (Design)
        Implement_Display // Implement Display trait (Design)
    Convert_to_Result // Convert to Result<T,E> (Design)
        Refactor_Core // Refactor tqp-core functions (Design)
        Refactor_IBM // Refactor tqp-ibm functions (Design)
    Handle_Edge_Cases // Handle edge cases (Design)
        Handle_OOM // Out of memory (Design)
        Handle_IBM_Timeout // IBM timeout (Design)
        Handle_NaN // NaN detection (Design)
```

### P2_API_Documentation (Decomposed)

```text
P2_API_Documentation // API Documentation (6/8 agreement) (Design)
    Generate_Rustdoc // Generate rustdoc (Design)
        Add_Doc_Comments // Comment all pub functions (Design)
        Add_Examples // Code examples (Design)
    Create_SDK_Guide // SDK Guide (Design)
        Quick_Start // Quick start (Design)
        API_Reference // API reference (Design)
```

### P2_Reproducibility (Decomposed)

```text
P2_Reproducibility // Ensure Reproducibility (5/8 agreement) (Design)
    Create_Dockerfile // Docker environment (Design)
        Base_Image // Rust base (Design)
        Install_Deps // Install dependencies (Design)
        Run_Tests // Test script (Design)
    Add_Seed_Control // Seed control (Design)
        Global_Seed // Global seed (Design)
        Apply_RNG // Apply RNG (Design)
    Repro_Checklist // Reproducibility checklist (Design)
```

### P3_GPU_PoC (Decomposed)

```text
P3_GPU_PoC // GPU PoC Implementation (7/8 agreement) (Design)
    Analyze_cuQuantum // Analyze cuQuantum API (Design)
        Check_Format // 3D tensor vs 1D vector (Design)
        Design_Flatten // L×M×2^N → 2^N (Design)
    Implement_Kernels // Implement CUDA kernels (Design)
        Hadamard_Kernel // H gate (Design)
        CNOT_Kernel // CNOT gate (Design)
    Benchmark_GPU // GPU vs CPU benchmark (Design)
        N20_N24_Compare // N=20-24 comparison (Design)
        Report_Speedup // Report speedup (Design)
```

### P3_Package_Deploy (Decomposed)

```text
P3_Package_Deploy // Package Deployment (6/8 agreement) (Design)
    Crates_io // Crates.io deployment (Design)
        Update_Cargo // Metadata (Design)
        Add_Badge // License badge (Design)
    PyPI // PyPI deployment (Design)
        PyO3_Bindings // Generate bindings (Design)
        Build_Wheel // Build wheel (Design)
```

---

## Execution Schedule (Unified)

| Phase | Duration | Work | Completion Criteria | Dependencies | Status |
|-------|----------|------|---------------------|--------------|--------|
| **Phase 0** | 1 week | Immediate fixes (7 items) | PRX submission minimum | None | ✅ Done |
| **Phase 1** | 3 weeks | PRX submission prep | Paper submission complete | Phase 0 | 🔄 InProgress |
| **Phase 2** | 2 months | Technical extension | N=30, BeH₂ validation | Phase 1 | Waiting |
| **Phase 3** | 3 months | Commercialization prep | GPU 10-50x | Phase 2 | Design |

### Detailed Schedule

```text
2025 Jan Week 1   ── Phase 0: Immediate Fixes (7 items) ✅ Done
2025 Jan Week 2-4 ── Phase 1: PRX Submission Prep 🔄 InProgress
  - Spinoza Benchmark ✅ Done (12/27)
  - H₂ Statistics ✅ Done (12/27)
  - TEX Sync 📌 Next Task
  - PDF Recompile 📌 Next Task
2025 Late Jan     ── ★ PRX Quantum Submission
2025 Feb-Mar      ── Phase 2: Technical Extension
2025 Apr-Jun      ── Phase 3: Commercialization Prep
2025 Q3           ── TQP v1.0 Official Release
```

---

## Success Metrics (Unified)

| Metric | Current v3.4 | After v3.5 | After Phase 1 | After Phase 2 | After Phase 3 |
|--------|--------------|------------|---------------|---------------|---------------|
| PRX Score | 55/100 | **70/100** | 80/100 | 85/100 | 90/100 |
| Reference Count | 21 | 22 | 25 | 25 | 30 |
| Benchmark Fairness | Stated | **✅ Spinoza** | ✅ | ✅ | ✅ |
| Hardware Validated Molecules | 1 | 1 (stats) | 1 | 2 | 2 |
| H₂ Error | -7.4 mHa | **-4.2 mHa** | -4.2 mHa | -4.2 mHa | -4.2 mHa |
| Max Qubits | N=24 | N=24 | N=24 | N=30 | N=34 |
| Spinoza Comparison | ❌ | **✅ 9-17×** | ✅ | ✅ | ✅ |
| GPU Acceleration | - | - | - | - | 10-50x |
| API Docs | None | None | None | rustdoc | SDK |

---

## Risk Management

| Risk | Impact | Mitigation Strategy | Status |
|------|--------|---------------------|--------|
| Fair benchmark results worsen | PRX rejection | Resolved with Spinoza comparison | ✅ Resolved |
| Python overhead controversy | Credibility loss | Performed Rust-to-Rust comparison | ✅ Resolved |
| BeH₂ hardware failure | Paper weakened | Substitute with H₂ multiple bond lengths | Waiting |
| Sparse implementation delay | Phase 2 delay | Use external library (nalgebra-sparse) | Waiting |
| GPU kernel conflicts | Phase 3 delay | Use cuQuantum directly (abandon 3D) | Waiting |

---

## Verification Plan

| Phase | Verification Method | Status |
|-------|---------------------|--------|
| Phase 0 | Document review (8 AI re-review) | ✅ Done |
| Phase 1 | `cargo test`, benchmark re-run, CI pass, **Spinoza comparison** | 🔄 InProgress |
| Phase 2 | N=30 test, BeH₂ IBM execution, `cargo doc` | Waiting |
| Phase 3 | GPU benchmark, `pip install tqp` verification | Waiting |

---

## Work Priority Summary

```text
✅ P0 (Done): Abstract, f(m), accuracy, Data Availability
🔄 P1 (InProgress): Spinoza benchmark ✅, H₂ stats ✅, TEX sync, PDF compile
🟡 P2 (2 months): Sparse, BeH₂, error handling, API, TQP optimization
🟢 P3 (3 months): GPU, packages, multi-backend
```

---

## Changelog

### v2.0 (2025-12-27)

- **Added:** P1_Spinoza_Benchmark complete subtree (Done)
- **Added:** P1_H2_Statistics subtree (Done)
- **Added:** P2_TQP_Optimization subtree (SoA, SIMD, Pipeline)
- **Updated:** Dependency analysis with Spinoza comparison
- **Updated:** Success metrics with Spinoza comparison, H₂ statistics
- **Updated:** Risk management - Python overhead controversy resolved
- **Updated:** Version v3.4 → v3.5
- **Updated:** Detailed schedule - Reflected 12/27 completed work
