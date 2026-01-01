# TQP v3.5 통합 업그레이드 작업계획 (Gantree)

**버전:** 2.0  
**작성일:** 2025-12-27  
**기반 리뷰:** TQP_Integrated_Review_Paper.md + TQP_Integrated_Review_Technical.md (8개 AI 통합)  
**목표:** PRX Quantum 투고 + Production-Ready 달성

---

## 설계 원칙

1. **PRX 투고 우선** - 논문/기술 작업 의존성 고려
2. **Top-Down BFS** 방식 설계
3. **원자화 노드**까지 분해 (AI가 15분 내 구현 가능 단위)
4. **5레벨 이상** 분해 시 별도 루트 분리

---

## 의존성 분석

```text
[논문 작업]              [기술 작업]                [의존성]
─────────────────────────────────────────────────────────────
벤치마크 공정성   ←──── E2E/Kernel 분리          (기술→논문) ✅ 완료
Spinoza 비교     ←──── tqp_vs_spinoza.rs 구현   (기술→논문) ✅ 완료
추가 분자 검증    ←──── BeH₂ IBM 실행            (기술→논문)
Data Availability ←──── GitHub 데이터셋 정리     (기술→논문) ✅ 완료
참고문헌 확대     ───── (독립)                   (논문 단독) ✅ 완료
Sparse 메모리     ───── (독립)                   (기술 단독)
GPU PoC          ───── (독립)                   (기술 단독)
```

---

## 통합 작업 트리

```text
TQP_v3.5_Unified_Upgrade // v3.5 통합 업그레이드 (진행중)
    Phase_0_Immediate // 즉시 수정 - PRX 투고 전 (1주) (완료)
        P0_Paper_Abstract // 논문 Abstract 수정 (완료)
            Add_Python_Overhead_Caveat // "includes Python overhead" 추가 (완료)
            Add_Crossover_Mention // N≥17 crossover 언급 (완료)
            Tone_Down_Speedup // "2-3000x" → "end-to-end" 재표현 (완료)
        P0_Tech_Benchmark_Caveat // 기술 사양서 caveat 추가 (완료)
            Add_Caveat_Text // Section 6 벤치마크 주석 추가 (완료)
            Fix_M_Scaling // "Near-linear (R²=0.98)" 표현 수정 (완료)
        P0_Tent_Definition // T̂_ent f(m) 정의 (완료)
            Verify_fm_Unitarity // f(m) 유니터리성 수학 증명 선행 (완료)
            Define_fm_XOR // f(m) = m XOR (1 << (m % n)) (완료)
            Add_N2M2_Example // N=2, M=2 예제 추가 (완료)
            Prove_Unitarity // 유니터리성 증명 논문 포함 (완료)
        P0_Hardware_Accuracy // 하드웨어 정확도 해석 수정 (완료)
            Get_FCI_Reference // FCI 값 출처 명시 (NIST/문헌) (완료)
            Change_Ref_to_FCI // HF→FCI 기준 변경 (완료)
            Add_ChemAcc_Warning // 1.6mHa 초과 명시 (완료)
        P0_Data_Availability // Data Availability Statement (완료)
            Write_DAS // Statement 작성 (완료)
            Prepare_GitHub_Raw // raw_data 폴더 준비 (완료)
        P0_Hint_Clarify // H_int 사용 여부 명확화 (완료)
            Check_Usage // 코드 내 H_int 사용 확인 (완료)
            Move_Future_Work // 미사용 시 "future work" 분류 (완료)
        P0_Backend_Unify // 백엔드 통일 (완료)
            Update_Appendix // ibm_torino로 통일 (완료)
    Phase_1_PRX_Ready // PRX 투고 준비 (3주) (진행중)
        P1_References_Expand // 참고문헌 확대 (완료)
            Add_21_References // 3개 → 21개 확대 (완료)
        P1_Fair_Benchmark // 공정 벤치마크 설계 및 실행 (완료)
            Design_Fair_Protocol // 공정 비교 프로토콜 (완료)
                Option_B_Warmup_Qiskit // 10회 warm-up 후 측정 (완료)
            Separate_E2E_Kernel // E2E vs Kernel 시간 분리 (완료)
                Measure_Python_Overhead // Python 오버헤드 측정 (완료)
            Run_Fair_Benchmark // 벤치마크 실행 (완료)
                Run_N14_to_N20 // N=14-20 30회 반복 (완료)
            Update_Paper // 논문 업데이트 (완료)
                Add_Section_3_4_1 // Methods Python Overhead 추가 (완료)
                Add_Section_5_3_1 // Discussion 해석 추가 (완료)
        P1_Spinoza_Benchmark // Spinoza Rust-to-Rust 비교 [NEW] (완료)
            Install_Nightly // Rust nightly 설치 (완료)
            Add_Spinoza_Dep // Cargo.toml 의존성 추가 (완료)
            Write_Benchmark // tqp_vs_spinoza.rs 작성 (완료)
            Run_Init_Benchmark // 상태 초기화 비교 (완료)
            Run_Gate_Benchmark // 게이트 연산 비교 (완료)
            Update_Paper_342 // Section 3.4.2 추가 (완료)
            Update_Limitations // Section 5.3 업데이트 (완료)
        P1_H2_Statistics // H₂ 통계 검증 [NEW] (완료)
            Run_5_Trials // 5회 반복 실행 (완료)
            Calculate_Mean // 평균 -4.2 mHa (완료)
            Update_Abstract // Abstract 값 업데이트 (완료)
            Update_Conclusion // Conclusion 값 업데이트 (완료)
        P1_Crossover_Precision // Crossover 분석 정밀화 (완료)
            Measure_N14_to_N20 // N=14-20 각 30회 측정 (완료)
            Report_Results // 결과 CSV 저장 (완료)
        P1_Time_Concept // "Time as Resource" 명확화 (설계중)
            Separate_Physics_DS // 물리 모델 vs 자료구조 분리 (설계중)
            Add_Photonic_Relation // time-bin photonic QC 관계 (설계중)
        P1_Tensor_Network // 텐서 네트워크 연결 구체화 (설계중)
            Define_MPS_Eq // TQP = MPS(χ≤2^M) 수식 (설계중)
        P1_PRX_Submit // PRX 투고 최종 준비 (진행중)
            Sync_TEX // TQP_PRX.tex ← draft_v1.md 동기화 (진행중)
            Compile_PDF // PDF 재컴파일 (진행중)
            Prepare_SM // Supplementary Material 준비 (진행중)
            Prepare_Rebuttal_Template // 리뷰어 대응 템플릿 (진행중)
            Final_Review // 최종 검토 (진행중)
            Submit_Draft // 초안 제출 (설계중)
    Phase_2_Extension // 기술 확장 - 투고 후 (2개월) (진행중)
        P2_External_Dependency // 외부 의존성 점검 (완료)
            Check_IBM_Quota // IBM 할당량 확인 (완료: 부족)
            Check_PySCF_Version // PySCF 버전 호환성 (완료)
        P2_BeH2_Validation // BeH₂ 14-qubit 검증 (완료: 시뮬레이션 한정)
            Generate_BeH2_Hamiltonian // 14-qubit 666항 생성 (완료)
            Add_to_Paper // Section 4.4, SM S5 추가 (완료)
            Run_IBM_Hardware // IBM 하드웨어 실행 (보류: Quota 부족)
        P2_Sparse_Memory // Sparse 메모리 구현 (분해)
        P2_Error_Handling // 에러 처리 체계화 (분해)
        P2_TQP_Optimization // TQP 성능 최적화 [NEW] (설계중)
            SoA_Memory_Refactor // Struct of Arrays 메모리 구조 (설계중)
            SIMD_Integration // AVX-512 SIMD 확장 (설계중)
            Temporal_Pipeline // 시간 축 파이프라인 병렬화 (설계중)
    Phase_3_Commercialization // 상용화 준비 (3개월) (설계중)
        P3_Resource_Estimate // 리소스 추정 (설계중)
            Estimate_FTE // 예상 인력 (FTE) (설계중)
            Estimate_Cost // 예상 비용 (설계중)
        P3_Milestone_Checkpoint // 2주 단위 채크포인트 (설계중)
        P3_GPU_PoC // GPU PoC 구현 (분해)
            GPU_Requirements // CUDA CC 7.0+, VRAM 8GB+ (설계중)
        P3_Package_Deploy // PyPI/Crates 배포 (분해)
        P3_Multi_Backend // 멀티 백엔드 (보류)
```

---

## 분해된 서브트리

### P1_Spinoza_Benchmark (완료) [NEW]

```text
P1_Spinoza_Benchmark // Spinoza Rust-to-Rust 비교 (완료)
    Install_Nightly // Rust nightly 1.94.0 설치 (완료)
    Add_Spinoza_Dep // Cargo.toml spinoza 의존성 추가 (완료)
    Write_Benchmark // tqp_vs_spinoza.rs 작성 (완료)
        Init_Benchmark // 상태 초기화 비교 코드 (완료)
        Gate_Benchmark // Hadamard, PauliX, 시퀀스 (완료)
    Run_Benchmark // cargo +nightly bench 실행 (완료)
    Analyze_Results // 결과 분석 (완료)
        Init_TQP_1_4_1_9x // 상태 초기화 TQP 1.4-1.9× 우위 (완료)
        Gate_TQP_9_17x // 게이트 연산 TQP 9-17× 우위 (완료)
        Crossover_N14_16 // Crossover point N≈14-16 (완료)
    Update_Paper // draft_v1.md 업데이트 (완료)
        Add_Section_342 // §3.4.2 추가 (완료)
        Update_Limitations // §5.3 경고 갱신 (완료)
    Update_Spec // TQP_Integrated_Specification_3.5.md 작성 (완료)
```

### P2_BeH2_Validation (분해)

```text
P2_BeH2_Validation // BeH₂ 14-qubit 하드웨어 검증 (설계중)
    Generate_BeH2_Hamiltonian // PySCF로 Hamiltonian 생성 (완료)
        Setup_PySCF // PySCF 환경 설정 (완료)
        Run_CASSCF // CASSCF(3,2) 계산 (완료)
        Convert_JordanWigner // Jordan-Wigner 변환 (완료)
    Run_IBM_Validation // IBM 하드웨어 실행 (보류)
        Prepare_QASM // QASM 3.0 생성 (설계중)
        Submit_Estimator // Estimator V2 제출 (설계중)
        Collect_Results // 결과 수집 (설계중)
    Apply_Error_Mitigation // Error mitigation 적용 (설계중)
        Apply_ZNE // Zero Noise Extrapolation (설계중)
        Report_Mitigated_Error // 완화된 오차 보고 (설계중)
    Update_Paper_Results // 논문 결과 업데이트 (설계중)
```

### P2_Sparse_Memory (분해)

```text
P2_Sparse_Memory // Sparse 메모리 구현 (8/8 동의) (설계중)
    Design_Sparse_Format // CSR/COO 포맷 선택 (설계중)
        Analyze_Sparsity_Pattern // TQP 상태 희소성 분석 (설계중)
        Select_Format // 패턴 기반 포맷 결정 (설계중)
    Implement_SparseState // SparseState 구조체 (설계중)
        Define_Struct // 필드 정의 (설계중)
        From_Dense // Dense→Sparse 변환 (설계중)
        To_Dense // Sparse→Dense 변환 (설계중)
    Implement_Sparse_Ops // Sparse 연산 구현 (설계중)
        Sparse_Gate // 희소 게이트 적용 (설계중)
        Sparse_FastMux // 희소 FastMux shift (설계중)
        Sparse_Measure // 희소 측정 (설계중)
    Dynamic_Policy // 동적 정책 선택기 (설계중)
        Calc_Memory // 메모리 요구량 계산 (설계중)
        Select_Auto // Dense/Sparse 자동 선택 (설계중)
    Test_N30 // N=30 테스트 on 64GB (설계중)
```

### P2_Error_Handling (분해)

```text
P2_Error_Handling // 에러 처리 체계화 (6/8 동의) (설계중)
    Define_TqpError // 에러 타입 정의 (설계중)
        Create_Enum // TqpError enum 생성 (설계중)
        Implement_Display // Display trait 구현 (설계중)
    Convert_to_Result // Result<T,E> 전환 (설계중)
        Refactor_Core // tqp-core 함수 리팩토링 (설계중)
        Refactor_IBM // tqp-ibm 함수 리팩토링 (설계중)
    Handle_Edge_Cases // Edge case 대응 (설계중)
        Handle_OOM // 메모리 부족 (설계중)
        Handle_IBM_Timeout // IBM 타임아웃 (설계중)
        Handle_NaN // NaN 감지 (설계중)
```

### P2_API_Documentation (분해)

```text
P2_API_Documentation // API 문서화 (6/8 동의) (설계중)
    Generate_Rustdoc // rustdoc 생성 (설계중)
        Add_Doc_Comments // 모든 pub 함수 주석 (설계중)
        Add_Examples // 코드 예제 (설계중)
    Create_SDK_Guide // SDK 가이드 (설계중)
        Quick_Start // 빠른 시작 (설계중)
        API_Reference // API 레퍼런스 (설계중)
```

### P2_Reproducibility (분해)

```text
P2_Reproducibility // 재현성 보장 (5/8 동의) (설계중)
    Create_Dockerfile // Docker 환경 (설계중)
        Base_Image // Rust 베이스 (설계중)
        Install_Deps // 의존성 설치 (설계중)
        Run_Tests // 테스트 스크립트 (설계중)
    Add_Seed_Control // 시드 제어 (설계중)
        Global_Seed // 전역 시드 (설계중)
        Apply_RNG // RNG 적용 (설계중)
    Repro_Checklist // 재현성 체크리스트 (설계중)
```

### P3_GPU_PoC (분해)

```text
P3_GPU_PoC // GPU PoC 구현 (7/8 동의) (설계중)
    Analyze_cuQuantum // cuQuantum API 분석 (설계중)
        Check_Format // 3D 텐서 vs 1D 벡터 (설계중)
        Design_Flatten // L×M×2^N → 2^N (설계중)
    Implement_Kernels // CUDA 커널 구현 (설계중)
        Hadamard_Kernel // H 게이트 (설계중)
        CNOT_Kernel // CNOT 게이트 (설계중)
    Benchmark_GPU // GPU vs CPU 벤치마크 (설계중)
        N20_N24_Compare // N=20-24 비교 (설계중)
        Report_Speedup // 가속비 보고 (설계중)
```

### P3_Package_Deploy (분해)

```text
P3_Package_Deploy // 패키지 배포 (6/8 동의) (설계중)
    Crates_io // Crates.io 배포 (설계중)
        Update_Cargo // 메타데이터 (설계중)
        Add_Badge // 라이선스 뱃지 (설계중)
    PyPI // PyPI 배포 (설계중)
        PyO3_Bindings // 바인딩 생성 (설계중)
        Build_Wheel // 휠 빌드 (설계중)
```

---

## 실행 일정 (통합)

| Phase | 기간 | 작업 | 완료 기준 | 의존성 | 상태 |
|-------|------|------|----------|--------|------|
| **Phase 0** | 1주 | 즉시 수정 7건 | PRX 투고 최소 조건 | 없음 | ✅ 완료 |
| **Phase 1** | 3주 | PRX 투고 준비 | 논문 제출 완료 | Phase 0 | 🔄 진행중 |
| **Phase 2** | 2개월 | 기술 확장 | N=30, BeH₂ 검증 | Phase 1 | 대기 |
| **Phase 3** | 3개월 | 상용화 준비 | GPU 10-50x | Phase 2 | 설계중 |

### 상세 일정

```text
2025년 1월 1주 ── Phase 0: 즉시 수정 (7건) ✅ 완료
2025년 1월 2-4주  ── Phase 1: PRX 투고 준비 🔄 진행중
  - Spinoza 벤치마크 ✅ 완료 (12/27)
  - H₂ 통계 검증 ✅ 완료 (12/27)
  - TEX 동기화 📌 다음 작업
  - PDF 재컴파일 📌 다음 작업
2025년 1월 말    ── ★ PRX Quantum 투고
2025년 2-3월    ── Phase 2: 기술 확장
2025년 4-6월    ── Phase 3: 상용화 준비
2025년 Q3      ── TQP v1.0 정식 출시
```

---

## 성공 지표 (통합)

| 지표 | 현재 v3.4 | v3.5 후 | Phase 1 후 | Phase 2 후 | Phase 3 후 |
|------|-----------|---------|------------|------------|------------|
| PRX 평가 점수 | 55/100 | **70/100** | 80/100 | 85/100 | 90/100 |
| 참고문헌 수 | 21개 | 22개 | 25개 | 25개 | 30개 |
| 벤치마크 공정성 | 명시 | **✅ Spinoza** | ✅ | ✅ | ✅ |
| 하드웨어 검증 분자 | 1개 | 1개 (통계) | 1개 | 2개 | 2개 |
| H₂ 오차 | -7.4 mHa | **-4.2 mHa** | -4.2 mHa | -4.2 mHa | -4.2 mHa |
| 최대 큐비트 | N=24 | N=24 | N=24 | N=30 | N=34 |
| Spinoza 비교 | ❌ | **✅ 9-17×** | ✅ | ✅ | ✅ |
| GPU 가속 | - | - | - | - | 10-50x |
| API 문서 | 없음 | 없음 | 없음 | rustdoc | SDK |

---

## 리스크 관리

| 리스크 | 영향 | 완화 전략 | 상태 |
|--------|------|----------|------|
| 공정 벤치마크 결과 악화 | PRX 거절 | Spinoza 비교로 해결 | ✅ 해결 |
| Python 오버헤드 논란 | 신뢰성 저하 | Rust-to-Rust 비교 수행 | ✅ 해결 |
| BeH₂ 하드웨어 실패 | 논문 약화 | H₂ 다중 결합 길이로 대체 | 대기 |
| Sparse 구현 지연 | Phase 2 지연 | 외부 라이브러리 활용 (nalgebra-sparse) | 대기 |
| GPU 커널 충돌 | Phase 3 지연 | cuQuantum 직접 사용 (3D 포기) | 대기 |

---

## 검증 계획

| Phase | 검증 방법 | 상태 |
|-------|----------|------|
| Phase 0 | 문서 리뷰 (8개 AI 재검토) | ✅ 완료 |
| Phase 1 | `cargo test`, 벤치마크 재실행, CI 통과, **Spinoza 비교** | 🔄 진행중 |
| Phase 2 | N=30 테스트, BeH₂ IBM 실행, `cargo doc` | 대기 |
| Phase 3 | GPU 벤치마크, `pip install tqp` 확인 | 대기 |

---

## 작업 우선순위 요약

```text
✅ P0 (완료): Abstract, f(m), 정확도, Data Availability
� P1 (진행중): Spinoza 벤치마크 ✅, H₂ 통계 ✅, TEX 동기화, PDF 컴파일
🟡 P2 (2개월): Sparse, BeH₂, 에러처리, API, TQP 최적화
🟢 P3 (3개월): GPU, 패키지, 멀티백엔드
```

---

## Changelog

### v2.0 (2025-12-27)

- **Added:** P1_Spinoza_Benchmark 전체 서브트리 (완료)
- **Added:** P1_H2_Statistics 서브트리 (완료)
- **Added:** P2_TQP_Optimization 서브트리 (SoA, SIMD, Pipeline)
- **Updated:** 의존성 분석에 Spinoza 비교 추가
- **Updated:** 성공 지표에 Spinoza 비교, H₂ 통계 반영
- **Updated:** 리스크 관리 - Python 오버헤드 논란 해결
- **Updated:** 버전 v3.4 → v3.5
- **Updated:** 상세 일정 - 12/27 작업 완료 반영
