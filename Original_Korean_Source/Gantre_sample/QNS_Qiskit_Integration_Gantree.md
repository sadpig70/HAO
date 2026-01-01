# QNS-Qiskit Integration Gantree Design

> **설계 방식**: PPR/Gantree 프레임워크 기반  
> **접근법**: Top-Down BFS, 원자화 노드까지 분해  
> **목표**: IBM Quantum 하드웨어 검증을 위한 시뮬레이션 우선 통합

---

## 📋 설계 개요

**최상위 목표**: QNS 프레임워크를 Qiskit과 통합하여 실제 IBM Quantum 하드웨어에서 노이즈 적응형 최적화 효과를 검증하되, 비용 최소화를 위해 Aer 시뮬레이션을 통한 사전 검증을 수행한다.

**핵심 전략**: Simulation-First Validation → Hardware Execution

---

## 🌲 Gantree 설계 트리

```
QNS_Qiskit_Integration // QNS-Qiskit 통합 시스템 (진행중)
    Phase1_SimulationIntegration // Aer 시뮬레이션 통합 (설계중)
        L1_PythonRustBridge // Python-Rust 브리지 구축 (설계중)
            CircuitConverter // 회로 변환 모듈 (설계중)
                QNSToQiskitTranslator // QNS CircuitGenome → Qiskit QuantumCircuit (설계중)
                    ParseQNSGates // QNS 게이트 파싱 (설계중)
                    MapToQiskitGates // Qiskit 게이트 매핑 (설계중)
                    BuildQuantumCircuit // QuantumCircuit 객체 생성 (설계중)
                QiskitToQNSConverter // Qiskit → QNS 역변환 (보류)
            PyO3Bindings // Rust-Python 바인딩 (설계중)
                ExportCircuitConversion // circuit 변환 함수 노출 (설계중)
                ExportCalibrationFetch // calibration 조회 함수 노출 (설계중)
                ExportSimulationRunner // 시뮬레이션 실행 함수 노출 (설계중)
        
        L2_CalibrationIntegration // 캘리브레이션 데이터 통합 (설계중)
            IBMBackendConnector // IBM 백엔드 연결 (설계중)
                AuthenticateService // QiskitRuntimeService 인증 (설계중)
                    LoadAPIKey // API Key 환경변수 로딩 (설계중)
                    InitializeService // Service 객체 초기화 (설계중)
                SelectBackend // 백엔드 선택 (설계중)
                    ListAvailableBackends // 사용 가능 백엔드 조회 (설계중)
                    FilterBySpecs // 큐비트 수/타입 필터링 (설계중)
            CalibrationDataFetcher // 캘리브레이션 데이터 조회 (설계중)
                FetchBackendProperties // backend.properties() 호출 (설계중)
                ParseT1T2Data // T1/T2 시간상수 파싱 (설계중)
                ParseGateErrors // 게이트 에러율 파싱 (설계중)
                ParseReadoutErrors // 측정 에러율 파싱 (설계중)
            NoiseModelBuilder // Qiskit NoiseModel 생성 (설계중)
                CreateNoiseModel // NoiseModel 객체 초기화 (설계중)
                AddT1T2Errors // 코히런스 에러 추가 (설계중)
                AddGateErrors // 게이트 에러 추가 (설계중)
                AddReadoutErrors // 측정 에러 추가 (설계중)
            QNSNoiseVectorAdapter // QNS NoiseVector 변환 (설계중)
                MapCalibrationToNoiseVector // Calibration → NoiseVector (설계중)
                ValidateNoiseVector // NoiseVector 유효성 검증 (설계중)
        
        L3_AerSimulation // Aer 노이즈 시뮬레이션 (설계중)
            SimulatorBackendFactory // 시뮬레이터 백엔드 생성 (설계중)
                CreateAerSimulator // AerSimulator 인스턴스 생성 (설계중)
                AttachNoiseModel // NoiseModel 연결 (설계중)
            CircuitExecutor // 회로 실행 엔진 (설계중)
                PrepareCircuit // 회로 준비 (측정 추가 등) (설계중)
                RunSimulation // Aer 시뮬레이션 실행 (설계중)
                    SetShots // shots 설정 (설계중)
                    ExecuteCircuit // circuit.run() 호출 (설계중)
                    WaitForResult // 결과 대기 (설계중)
                ParseResults // 결과 파싱 (설계중)
                    ExtractCounts // 측정 카운트 추출 (설계중)
                    CalculateFidelity // 피델리티 계산 (설계중)
            ComparisonEngine // QNS vs 비최적화 비교 (설계중)
                RunIdentityMapping // Identity 매핑 회로 실행 (설계중)
                RunQNSOptimized // QNS 최적화 회로 실행 (설계중)
                ComputeFidelityDelta // 피델리티 차이 계산 (설계중)
                GenerateReport // 비교 리포트 생성 (설계중)
        
        L4_CLIIntegration // CLI 통합 (설계중)
            CLIBackendSelector // 백엔드 선택 로직 (설계중)
                ParseBackendOption // --backend 옵션 파싱 (설계중)
                ValidateBackendType // 백엔드 타입 검증 (설계중)
            QiskitRunnerModule // Qiskit 실행 모듈 (설계중)
                InitializeRunner // QiskitRunner 초기화 (설계중)
                ExecuteWithBackend // 백엔드별 실행 분기 (설계중)
                    RunSimulatorMode // Simulator 모드 실행 (설계중)
                    RunAerNoisyMode // AerNoisy 모드 실행 (설계중)
                    RunIBMMode // IBM Hardware 모드 (보류) (설계중)
                FormatOutput // 결과 포맷팅 (설계중)
    
    Phase2_HardwareIntegration // IBM 하드웨어 통합 (설계중)
        L1_IBMRuntimeIntegration // Qiskit Runtime 통합 (설계중)
            RuntimeServiceManager // Runtime Service 관리 (설계중)
                CreateSession // Session 생성 (설계중)
                SelectHardwareBackend // 실제 하드웨어 선택 (설계중)
            JobSubmitter // Job 제출 엔진 (설계중)
                PrepareJob // Job 준비 (설계중)
                    TranspileCircuit // 회로 트랜스파일 (설계중)
                    SetExecutionOptions // 실행 옵션 설정 (설계중)
                SubmitJob // Job 제출 (설계중)
                MonitorJobStatus // Job 상태 모니터링 (설계중)
                    CheckQueuePosition // 큐 위치 확인 (설계중)
                    WaitForCompletion // 완료 대기 (설계중)
                RetrieveResults // 결과 수신 (설계중)
            ErrorHandler // 에러 처리 (설계중)
                HandleQueueTimeout // 큐 타임아웃 처리 (설계중)
                HandleJobFailure // Job 실패 처리 (설계중)
                RetryLogic // 재시도 로직 (설계중)
        
        L2_ValidationScripts // 하드웨어 검증 스크립트 (설계중)
            BenchmarkCircuitSet // 벤치마크 회로 세트 (설계중)
                BellStateCircuit // Bell State 회로 (설계중)
                GHZCircuit // GHZ State 회로 (설계중)
                QFTCircuit // QFT 회로 (설계중)
                CustomCircuits // 사용자 정의 회로 (설계중)
            ComparativeValidator // QNS vs. Qiskit 비교 (설계중)
                RunQiskitTranspiler // Qiskit 기본 트랜스파일 (설계중)
                RunQNSOptimizer // QNS 최적화 (설계중)
                ExecuteBothOnHardware // 양쪽 모두 하드웨어 실행 (설계중)
                StatisticalAnalysis // 통계 분석 (설계중)
                    CalculateMeanFidelity // 평균 피델리티 계산 (설계중)
                    PerformTTest // t-test 수행 (설계중)
                    ComputeEffectSize // Effect size (Cohen's d) 계산 (설계중)
            ResultCollector // 결과 수집 및 저장 (설계중)
                SaveToJSON // JSON 파일 저장 (설계중)
                SaveToCSV // CSV 파일 저장 (설계중)
                GenerateVisualization // 시각화 생성 (설계중)
    
    CrossCutting_Components // 공통 컴포넌트 (설계중)
        LoggingSystem // 로깅 시스템 (설계중)
            SetupLogger // 로거 설정 (설계중)
            LogCircuitInfo // 회로 정보 로깅 (설계중)
            LogExecutionMetrics // 실행 메트릭 로깅 (설계중)
        ConfigurationManager // 설정 관리 (설계중)
            LoadConfig // 설정 파일 로딩 (설계중)
            ValidateConfig // 설정 검증 (설계중)
            MergeWithDefaults // 기본값 병합 (설계중)
        TestSuite // 테스트 스위트 (설계중)
            UnitTests // 단위 테스트 (설계중)
                TestCircuitConversion // 회로 변환 테스트 (설계중)
                TestNoiseModelCreation // NoiseModel 생성 테스트 (설계중)
            IntegrationTests // 통합 테스트 (설계중)
                TestEndToEndSimulation // E2E 시뮬레이션 테스트 (설계중)
                TestHardwareConnection // 하드웨어 연결 테스트 (보류) (설계중)
```

---

## 🎯 원자화 노드 분석

### ✅ 이미 원자화된 노드 (직접 구현 가능)

| 노드 | 이유 | 예상 구현 시간 |
|------|------|---------------|
| `LoadAPIKey` | 환경변수 1줄 읽기 | 5분 |
| `SetShots` | 단일 파라미터 설정 | 5분 |
| `ExtractCounts` | 딕셔너리 추출 | 10분 |
| `SaveToJSON` | 표준 라이브러리 사용 | 10분 |
| `SaveToCSV` | pandas 1줄 | 10분 |

### ⚠️ 추가 분해 필요 노드

| 노드 | 분해 필요 이유 | 제안 |
|------|---------------|------|
| `RunSimulation` | "준비 → 실행 → 대기" 3단계 포함 | 이미 분해됨 (SetShots, ExecuteCircuit, WaitForResult) |
| `StatisticalAnalysis` | "계산 → 검정 → 효과크기" 3가지 통계 | 이미 분해됨 (하위 노드 3개) |

### 🟢 분해 완료 확인

현재 Gantree 구조는 **레벨 5 이하** 유지, 대부분 노드가 **원자화 수준**에 도달했습니다.

---

## 🔧 PPR 구현 예시 (Phase 1.1 - CircuitConverter)

```python
class AI_CircuitConverter:
    """Gantree: L1_PythonRustBridge → CircuitConverter"""
    
    def AI_make_qns_to_qiskit_translator(self, qns_circuit):
        """
        Gantree: QNSToQiskitTranslator
        Sub-nodes: ParseQNSGates → MapToQiskitGates → BuildQuantumCircuit
        """
        # ParseQNSGates (원자화 노드)
        gates = self._parse_qns_gates(qns_circuit)
        
        # MapToQiskitGates (원자화 노드)
        qiskit_gates = self._map_to_qiskit_gates(gates)
        
        # BuildQuantumCircuit (원자화 노드)
        qc = self._build_quantum_circuit(qiskit_gates, qns_circuit.num_qubits)
        
        return qc
    
    def _parse_qns_gates(self, circuit):
        """원자화 노드: QNS 게이트 리스트 파싱"""
        return [self._parse_single_gate(g) for g in circuit.gates]
    
    def _parse_single_gate(self, gate):
        """원자화: 단일 게이트 파싱 (15줄 이내)"""
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
        """원자화 노드: Qiskit 게이트 매핑"""
        from qiskit.circuit.library import HGate, CXGate, XGate, RZGate
        
        qiskit_map = {
            "h": HGate,
            "cx": CXGate,
            "x": XGate,
            "rz": RZGate,
        }
        
        return [(qiskit_map[g["type"]], g["qubits"], g["params"]) for g in gates]
    
    def _build_quantum_circuit(self, qiskit_gates, num_qubits):
        """원자화 노드: QuantumCircuit 생성"""
        from qiskit import QuantumCircuit
        
        qc = QuantumCircuit(num_qubits)
        for gate_class, qubits, params in qiskit_gates:
            qc.append(gate_class(*params), qubits)
        
        return qc
```

---

## 📊 구현 우선순위 (BFS 레벨 순서)

### Priority 1: Phase1 L1 (Python-Rust Bridge)

- **목표**: 회로 변환 및 PyO3 바인딩 완성
- **예상 시간**: 4-6시간
- **완료 조건**: Bell state가 Qiskit으로 변환되어 Aer에서 실행됨

### Priority 2: Phase1 L2 (Calibration Integration)

- **목표**: IBM 캘리브레이션 데이터 → NoiseModel 생성
- **예상 시간**: 3-4시간
- **완료 조건**: `ibm_fez`의 캘리브레이션으로 NoiseModel 생성 성공

### Priority 3: Phase1 L3 (Aer Simulation)

- **목표**: 노이즈 시뮬레이션 및 QNS 효과 검증
- **예상 시간**: 5-6시간
- **완료 조건**: Identity vs QNS 비교 시 +5~10% 피델리티 향상 확인

### Priority 4: Phase1 L4 (CLI Integration)

- **목표**: CLI `--backend aer-noisy` 옵션 추가
- **예상 시간**: 2-3시간
- **완료 조건**: `qns run --backend aer-noisy circuit.qasm` 실행 성공

### Priority 5: Phase2 (Hardware Integration)

- **목표**: 실제 IBM 하드웨어 실행
- **예상 시간**: 10-13시간
- **완료 조건**: 최소 5개 회로에서 통계적으로 유의한 피델리티 향상

---

## 🧪 검증 체크리스트

### Phase 1 검증

- [ ] CircuitConverter가 모든 QNS 게이트 타입을 Qiskit으로 변환
- [ ] NoiseModel이 실제 IBM 캘리브레이션 데이터 반영
- [ ] Aer 시뮬레이션 피델리티가 해석 가능한 범위 (0-1)
- [ ] CLI가 `simulator` / `aer-noisy` 백엔드 선택 지원

### Phase 2 검증

- [ ] IBM 하드웨어 Job이 큐에 정상 제출됨
- [ ] 결과 수신 및 파싱 성공
- [ ] QNS vs. Qiskit 비교 통계 분석 완료 (p-value, effect size)
- [ ] 결과가 JSON/CSV로 저장됨

---

## 📝 다음 단계

1. **사용자 승인**: 본 Gantree 설계 리뷰
2. **Priority 1 구현 시작**: `CircuitConverter` 원자화 노드부터 구현
3. **단위 테스트 작성**: 각 원자화 노드별 테스트
4. **점진적 통합**: L1 → L2 → L3 → L4 순차 통합

---

*설계 프레임워크: PPR/Gantree V4*  
*설계 방식: Top-Down BFS, 원자화 노드 분해*  
*예상 전체 구현 시간: 22-29시간 (Phase 1: 12-16h, Phase 2: 10-13h)*
