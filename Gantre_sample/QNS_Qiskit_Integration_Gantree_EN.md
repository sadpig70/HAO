# QNS-Qiskit Integration Gantree Design

> **Design Framework**: PPR/Gantree Framework  
> **Approach**: Top-Down BFS, Decomposition to Atomic Nodes  
> **Goal**: Simulation-first validation for IBM Quantum hardware verification

---

## 📋 Design Overview

**Top-Level Goal**: Integrate QNS framework with Qiskit to verify noise-adaptive optimization effects on real IBM Quantum hardware, with cost minimization through Aer simulation pre-validation.

**Core Strategy**: Simulation-First Validation → Hardware Execution

---

## 🌲 Gantree Design Tree

```
QNS_Qiskit_Integration // QNS-Qiskit Integration System (InProgress)
    Phase1_SimulationIntegration // Aer Simulation Integration (Design)
        L1_PythonRustBridge // Python-Rust Bridge Construction (Design)
            CircuitConverter // Circuit Conversion Module (Design)
                QNSToQiskitTranslator // QNS CircuitGenome → Qiskit QuantumCircuit (Design)
                    ParseQNSGates // Parse QNS Gates (Design)
                    MapToQiskitGates // Map to Qiskit Gates (Design)
                    BuildQuantumCircuit // Build QuantumCircuit Object (Design)
                QiskitToQNSConverter // Qiskit → QNS Reverse Conversion (Hold)
            PyO3Bindings // Rust-Python Bindings (Design)
                ExportCircuitConversion // Expose Circuit Conversion Function (Design)
                ExportCalibrationFetch // Expose Calibration Fetch Function (Design)
                ExportSimulationRunner // Expose Simulation Runner Function (Design)
        
        L2_CalibrationIntegration // Calibration Data Integration (Design)
            IBMBackendConnector // IBM Backend Connection (Design)
                AuthenticateService // QiskitRuntimeService Authentication (Design)
                    LoadAPIKey // Load API Key from Environment Variable (Design)
                    InitializeService // Initialize Service Object (Design)
                SelectBackend // Select Backend (Design)
                    ListAvailableBackends // List Available Backends (Design)
                    FilterBySpecs // Filter by Qubit Count/Type (Design)
            CalibrationDataFetcher // Calibration Data Fetcher (Design)
                FetchBackendProperties // Call backend.properties() (Design)
                ParseT1T2Data // Parse T1/T2 Time Constants (Design)
                ParseGateErrors // Parse Gate Error Rates (Design)
                ParseReadoutErrors // Parse Readout Error Rates (Design)
            NoiseModelBuilder // Build Qiskit NoiseModel (Design)
                CreateNoiseModel // Initialize NoiseModel Object (Design)
                AddT1T2Errors // Add Coherence Errors (Design)
                AddGateErrors // Add Gate Errors (Design)
                AddReadoutErrors // Add Readout Errors (Design)
            QNSNoiseVectorAdapter // QNS NoiseVector Adapter (Design)
                MapCalibrationToNoiseVector // Calibration → NoiseVector (Design)
                ValidateNoiseVector // Validate NoiseVector (Design)
        
        L3_AerSimulation // Aer Noise Simulation (Design)
            SimulatorBackendFactory // Simulator Backend Factory (Design)
                CreateAerSimulator // Create AerSimulator Instance (Design)
                AttachNoiseModel // Attach NoiseModel (Design)
            CircuitExecutor // Circuit Execution Engine (Design)
                PrepareCircuit // Prepare Circuit (Add Measurements, etc.) (Design)
                RunSimulation // Run Aer Simulation (Design)
                    SetShots // Set Shots (Design)
                    ExecuteCircuit // Call circuit.run() (Design)
                    WaitForResult // Wait for Result (Design)
                ParseResults // Parse Results (Design)
                    ExtractCounts // Extract Measurement Counts (Design)
                    CalculateFidelity // Calculate Fidelity (Design)
            ComparisonEngine // QNS vs Non-Optimized Comparison (Design)
                RunIdentityMapping // Run Identity Mapping Circuit (Design)
                RunQNSOptimized // Run QNS Optimized Circuit (Design)
                ComputeFidelityDelta // Compute Fidelity Difference (Design)
                GenerateReport // Generate Comparison Report (Design)
        
        L4_CLIIntegration // CLI Integration (Design)
            CLIBackendSelector // Backend Selection Logic (Design)
                ParseBackendOption // Parse --backend Option (Design)
                ValidateBackendType // Validate Backend Type (Design)
            QiskitRunnerModule // Qiskit Runner Module (Design)
                InitializeRunner // Initialize QiskitRunner (Design)
                ExecuteWithBackend // Execute by Backend Type (Design)
                    RunSimulatorMode // Run Simulator Mode (Design)
                    RunAerNoisyMode // Run AerNoisy Mode (Design)
                    RunIBMMode // Run IBM Hardware Mode (Hold) (Design)
                FormatOutput // Format Output (Design)
    
    Phase2_HardwareIntegration // IBM Hardware Integration (Design)
        L1_IBMRuntimeIntegration // Qiskit Runtime Integration (Design)
            RuntimeServiceManager // Runtime Service Manager (Design)
                CreateSession // Create Session (Design)
                SelectHardwareBackend // Select Real Hardware (Design)
            JobSubmitter // Job Submission Engine (Design)
                PrepareJob // Prepare Job (Design)
                    TranspileCircuit // Transpile Circuit (Design)
                    SetExecutionOptions // Set Execution Options (Design)
                SubmitJob // Submit Job (Design)
                MonitorJobStatus // Monitor Job Status (Design)
                    CheckQueuePosition // Check Queue Position (Design)
                    WaitForCompletion // Wait for Completion (Design)
                RetrieveResults // Retrieve Results (Design)
            ErrorHandler // Error Handler (Design)
                HandleQueueTimeout // Handle Queue Timeout (Design)
                HandleJobFailure // Handle Job Failure (Design)
                RetryLogic // Retry Logic (Design)
        
        L2_ValidationScripts // Hardware Validation Scripts (Design)
            BenchmarkCircuitSet // Benchmark Circuit Set (Design)
                BellStateCircuit // Bell State Circuit (Design)
                GHZCircuit // GHZ State Circuit (Design)
                QFTCircuit // QFT Circuit (Design)
                CustomCircuits // Custom Circuits (Design)
            ComparativeValidator // QNS vs. Qiskit Comparison (Design)
                RunQiskitTranspiler // Run Qiskit Default Transpiler (Design)
                RunQNSOptimizer // Run QNS Optimizer (Design)
                ExecuteBothOnHardware // Execute Both on Hardware (Design)
                StatisticalAnalysis // Statistical Analysis (Design)
                    CalculateMeanFidelity // Calculate Mean Fidelity (Design)
                    PerformTTest // Perform t-test (Design)
                    ComputeEffectSize // Compute Effect Size (Cohen's d) (Design)
            ResultCollector // Result Collection and Storage (Design)
                SaveToJSON // Save to JSON File (Design)
                SaveToCSV // Save to CSV File (Design)
                GenerateVisualization // Generate Visualization (Design)
    
    CrossCutting_Components // Cross-Cutting Components (Design)
        LoggingSystem // Logging System (Design)
            SetupLogger // Setup Logger (Design)
            LogCircuitInfo // Log Circuit Info (Design)
            LogExecutionMetrics // Log Execution Metrics (Design)
        ConfigurationManager // Configuration Manager (Design)
            LoadConfig // Load Config File (Design)
            ValidateConfig // Validate Config (Design)
            MergeWithDefaults // Merge with Defaults (Design)
        TestSuite // Test Suite (Design)
            UnitTests // Unit Tests (Design)
                TestCircuitConversion // Test Circuit Conversion (Design)
                TestNoiseModelCreation // Test NoiseModel Creation (Design)
            IntegrationTests // Integration Tests (Design)
                TestEndToEndSimulation // Test E2E Simulation (Design)
                TestHardwareConnection // Test Hardware Connection (Hold) (Design)
```

---

## 🎯 Atomic Node Analysis

### ✅ Already Atomic Nodes (Directly Implementable)

| Node | Reason | Estimated Time |
|------|--------|----------------|
| `LoadAPIKey` | Single line env var read | 5 min |
| `SetShots` | Single parameter setting | 5 min |
| `ExtractCounts` | Dictionary extraction | 10 min |
| `SaveToJSON` | Standard library usage | 10 min |
| `SaveToCSV` | Single pandas line | 10 min |

### ⚠️ Nodes Requiring Further Decomposition

| Node | Reason | Suggestion |
|------|--------|------------|
| `RunSimulation` | Contains "Prepare → Execute → Wait" 3 steps | Already decomposed (SetShots, ExecuteCircuit, WaitForResult) |
| `StatisticalAnalysis` | Contains "Calculate → Test → Effect Size" 3 statistics | Already decomposed (3 child nodes) |

### 🟢 Decomposition Complete

Current Gantree structure maintains **Level 5 or below**, with most nodes reaching **atomic level**.

---

## 🔧 PPR Implementation Example (Phase 1.1 - CircuitConverter)

```python
class AI_CircuitConverter:
    """Gantree: L1_PythonRustBridge → CircuitConverter"""
    
    def AI_make_qns_to_qiskit_translator(self, qns_circuit):
        """
        Gantree: QNSToQiskitTranslator
        Sub-nodes: ParseQNSGates → MapToQiskitGates → BuildQuantumCircuit
        """
        # ParseQNSGates (Atomic Node)
        gates = self._parse_qns_gates(qns_circuit)
        
        # MapToQiskitGates (Atomic Node)
        qiskit_gates = self._map_to_qiskit_gates(gates)
        
        # BuildQuantumCircuit (Atomic Node)
        qc = self._build_quantum_circuit(qiskit_gates, qns_circuit.num_qubits)
        
        return qc
    
    def _parse_qns_gates(self, circuit):
        """Atomic Node: Parse QNS gate list"""
        return [self._parse_single_gate(g) for g in circuit.gates]
    
    def _parse_single_gate(self, gate):
        """Atomic: Parse single gate (under 15 lines)"""
        gate_map = {
            "H": ("h", 1),
            "CNOT": ("cx", 2),
            "X": ("x", 1),
            "RZ": ("rz", 1),
            # ...
        }
        gate_type, num_qubits = gate_map.get(gate.name, (None, 0))
        return {
            "type": gate_type,
            "qubits": gate.qubits,
            "params": gate.params if hasattr(gate, 'params') else []
        }
    
    def _map_to_qiskit_gates(self, gates):
        """Atomic Node: Map to Qiskit gates"""
        from qiskit.circuit.library import HGate, CXGate, XGate, RZGate
        
        qiskit_map = {
            "h": HGate,
            "cx": CXGate,
            "x": XGate,
            "rz": RZGate,
        }
        
        return [(qiskit_map[g["type"]], g["qubits"], g["params"]) for g in gates]
    
    def _build_quantum_circuit(self, qiskit_gates, num_qubits):
        """Atomic Node: Build QuantumCircuit"""
        from qiskit import QuantumCircuit
        
        qc = QuantumCircuit(num_qubits)
        for gate_class, qubits, params in qiskit_gates:
            qc.append(gate_class(*params), qubits)
        
        return qc
```

---

## 📊 Implementation Priority (BFS Level Order)

### Priority 1: Phase1 L1 (Python-Rust Bridge)

- **Goal**: Complete circuit conversion and PyO3 bindings
- **Estimated Time**: 4-6 hours
- **Completion Criteria**: Bell state converted to Qiskit and executed on Aer

### Priority 2: Phase1 L2 (Calibration Integration)

- **Goal**: IBM calibration data → NoiseModel generation
- **Estimated Time**: 3-4 hours
- **Completion Criteria**: NoiseModel successfully created from `ibm_fez` calibration

### Priority 3: Phase1 L3 (Aer Simulation)

- **Goal**: Noise simulation and QNS effect verification
- **Estimated Time**: 5-6 hours
- **Completion Criteria**: +5~10% fidelity improvement in Identity vs QNS comparison

### Priority 4: Phase1 L4 (CLI Integration)

- **Goal**: Add CLI `--backend aer-noisy` option
- **Estimated Time**: 2-3 hours
- **Completion Criteria**: `qns run --backend aer-noisy circuit.qasm` executes successfully

### Priority 5: Phase2 (Hardware Integration)

- **Goal**: Real IBM hardware execution
- **Estimated Time**: 10-13 hours
- **Completion Criteria**: Statistically significant fidelity improvement on at least 5 circuits

---

## 🧪 Verification Checklist

### Phase 1 Verification

- [ ] CircuitConverter converts all QNS gate types to Qiskit
- [ ] NoiseModel reflects actual IBM calibration data
- [ ] Aer simulation fidelity is in interpretable range (0-1)
- [ ] CLI supports `simulator` / `aer-noisy` backend selection

### Phase 2 Verification

- [ ] IBM hardware Job successfully submitted to queue
- [ ] Results received and parsed successfully
- [ ] QNS vs. Qiskit statistical analysis complete (p-value, effect size)
- [ ] Results saved to JSON/CSV

---

## 📝 Next Steps

1. **User Approval**: Review this Gantree design
2. **Start Priority 1 Implementation**: Begin with `CircuitConverter` atomic nodes
3. **Write Unit Tests**: Tests for each atomic node
4. **Incremental Integration**: L1 → L2 → L3 → L4 sequential integration

---

*Design Framework: PPR/Gantree V4*  
*Design Approach: Top-Down BFS, Atomic Node Decomposition*  
*Estimated Total Implementation Time: 22-29 hours (Phase 1: 12-16h, Phase 2: 10-13h)*
