# Three-Layer Metal-Gate VCAT

## 프로젝트 개요 분석

**TCAD-Based Design Validation and Process Robustness Analysis of a Vertical-Channel Transistor with a Three-Layer Metal Gate**

고집적 DRAM의 4F² 구조를 위한 수직채널 트랜지스터(VCAT)를 대상으로, **Low–High–Low(L-H-L) Three-Layer Work-Function Gate**를 단계적으로 설계·검증하고 게이트 분할 경계의 형상 변동에 대한 **device-level tolerance window**를 분석한 공동 연구입니다.

본 연구는 Three-Layer WF gate 구조 자체를 새롭게 제안하는 것이 아니라, 선행 연구에서 제시된 L-H-L 구조를 기준으로 **일함수 조합 선정 → 게이트 구간 형상 탐색 → 2D/3D 비교 → 경계 변동 검증**을 수행하여 실제 설계에서 사용할 수 있는 nominal geometry와 허용 범위를 정량화하는 데 초점을 두었습니다.

**Summary:**  
This collaborative study uses Synopsys Sentaurus TCAD to validate a three-layer low–high–low work-function VCAT, select a balanced nominal geometry, compare 2D axisymmetric and full-3D results, and quantify a device-level gate-segmentation tolerance window around the nominal design.

| Item | Description |
|---|---|
| Device | Vertical-Channel Access Transistor (VCAT) for DRAM |
| Gate concept | Three-Layer Work-Function Gate, Low–High–Low |
| Metal stack | Ti / TiN / Ti = 4.33 / 4.70 / 4.33 eV |
| TCAD | Synopsys Sentaurus SDE / SDevice |
| Main variables | Xbnd1, Xbnd2 |
| Nominal geometry | Xbnd1 / Xbnd2 = 35 / 67 nm |
| Gate segmentation | M1 / M2 / M3 = 15 / 32 / 13 nm |
| Main metrics | DIBL, Ion, Ioff, Ion/Ioff, SS, GIDL |
| Robustness study | 46 geometries, 138 SDevice runs |
| Research type | Collaborative research |

> 상세 코드, Phase별 연구 기록과 원시 결과는 아래 공동 연구 저장소에서 확인할 수 있습니다.  
> [VCAT-1T1C-DRAM-TCAD-Research](https://github.com/jujushmaterial/VCAT-1T1C-DRAM-TCAD-Research)

---

## 연구 배경

DRAM이 고집적화되면서 기존 6F² 기반 access transistor 구조보다 작은 cell footprint를 구현하기 위한 4F² VCAT 구조가 연구되고 있습니다. VCAT은 channel을 수직 pillar 방향으로 형성하여 면적 축소에 유리하지만, 소자 설계에서는 서로 상충하는 전기적 요구를 동시에 만족해야 합니다.

- 높은 구동전류 `Ion`
- 낮은 누설전류 `Ioff`
- 작은 DIBL 및 안정적인 subthreshold 특성
- storage-node 측 BTBT에 의한 GIDL 억제
- floating-body 환경에서의 electrostatic stability

특히 gate work function을 channel 방향으로 분할하는 multi-WF 구조는 위치에 따라 barrier와 electric field를 조절할 수 있어 이러한 trade-off를 완화할 수 있는 설계 수단으로 활용됩니다.

---

## 문제 정의

선행 연구에서는 channel 양단에 Low-WF, 중앙에 High-WF를 배치하는 Three-Layer WF gate가 VCAT의 floating-body effect와 retention degradation을 억제할 수 있음을 제시했습니다.

하지만 실제 설계 관점에서는 다음 문제가 남습니다.

1. 여러 금속 후보 중 어떤 Low/High-WF 조합을 선택할 것인가?
2. 세 금속 구간의 경계 위치를 어떤 geometry로 설정할 것인가?
3. 선정된 nominal geometry가 경계 위치의 변동에도 성능을 유지하는가?
4. 계산 효율을 위해 사용한 2D axisymmetric 결과가 full-3D에서도 같은 방향성을 보이는가?

따라서 본 연구의 핵심은 단일 최고 성능점의 탐색이 아니라, **성능 균형을 갖는 nominal 구조의 선정과 그 주변 형상 변동에 대한 강건성 정량화**입니다.

---

## 선행 연구 분석

Three-Layer WF VCAT의 기본 구조는 선행 연구의 L-H-L gate 개념을 기반으로 합니다. 양단 Low-WF 영역은 junction 부근의 GIDL을 억제하고, 중앙 High-WF 영역은 channel depletion과 electrostatic control을 유지하는 역할을 갖습니다.

또한 위치 선택적 work-function engineering 연구에서는 VCT의 서로 다른 junction 위치가 독립적인 전기적 역할을 가지므로, work function의 공간적 배치가 leakage와 carrier injection을 각각 조절할 수 있음을 보여줍니다.

본 연구는 이러한 선행 결과를 새로운 구조 제안으로 재주장하지 않고, **실제 설계 변수 선정과 gate-segmentation geometry tolerance**로 연구 범위를 확장했습니다.

---

## 연구 방식 분석

<p align="center">
  <img src="assets/figures/research_workflow.webp" width="900" alt="Six-step research workflow">
</p>

연구는 한 번에 모든 변수를 최적화하지 않고, 앞 단계의 결과를 다음 단계의 입력으로 고정하는 단계적 방식으로 구성했습니다.

```text
Dual-WF 예비 검증
        ↓
Single-Metal VCAT 기준 소자
        ↓
Low/High Work-Function 조합
        ↓
Xbnd1–Xbnd2 형상 탐색
        ↓
2D Axisymmetric–Full 3D 대조
        ↓
Nominal 주변 형상 변동
        ↓
Tolerance Window
```

이 방식은 구조의 물리적 타당성, 재료 선택, geometry 선택, 모델 차원성, 형상 변동을 서로 분리하여 해석하기 위한 것입니다.

---

## 물리적 타당성 분석

<sub>Research record: preliminary Dual-WF validation</sub>

<table>
<tr>
<td width="50%"><img src="assets/figures/physical_feasibility_idvg.webp" alt="Dual-WF Id-Vg comparison"></td>
<td width="50%"><img src="assets/figures/physical_feasibility_cbe.webp" alt="Dual-WF conduction band comparison"></td>
</tr>
</table>

Dual-WF planar 시험 구조에서 LL, LH, HL, HH의 work-function 배치를 비교하여 gate WF의 크기와 공간적 배치가 channel electrostatics에 미치는 영향을 확인했습니다.

High-WF가 증가할수록 channel barrier와 turn-on voltage가 증가하는 방향이 나타났고, 평균 WF가 유사하더라도 High/Low-WF의 배치 순서에 따라 carrier injection 특성이 달라졌습니다. 이 결과는 Three-Layer gate 분석에서 중앙과 양단의 work function을 독립적으로 다룰 수 있는 물리적 근거로 사용했습니다.

> 이 단계는 본 VCAT baseline과 구조·치수·bias가 다른 preliminary test이며, 수치 자체를 이후 VCAT 결과에 직접 일반화하지 않습니다.

---

## 기준 소자 분석

<sub>Research record: Single-Metal VCAT baseline</sub>

<table>
<tr>
<td width="50%"><img src="assets/figures/baseline_doping_profile.webp" alt="Baseline VCAT doping profile"></td>
<td width="50%"><img src="assets/figures/baseline_idvg.webp" alt="Baseline VCAT transfer characteristics"></td>
</tr>
</table>

Three-Layer 구조의 성능을 비교하기 위한 기준으로 **TiN 4.70 eV Single-Metal VCAT**을 설정했습니다. 이후 비교에서 gate length, oxide thickness, doping profile과 junction 위치를 동일하게 유지하여 work-function segmentation과 geometry 변화의 영향을 분리했습니다.

기준 구조의 주요 조건은 다음과 같습니다.

| Parameter | Value |
|---|---:|
| Silicon pillar diameter | 12 nm |
| Gate oxide thickness | 1 nm |
| Gate length | 60 nm |
| Body doping | p-type 1×10¹⁷ cm⁻³ |
| S/D peak doping | n-type 1×10²⁰ cm⁻³ |
| Single-Metal WF | 4.70 eV |
| Body contact | Floating body |
| Temperature | 300 K |

---

## 일함수 조합 분석

<sub>Research record: Low/High-WF material screening</sub>

<p align="center">
  <img src="assets/figures/workfunction_pair_summary.webp" width="900" alt="Ten work-function pair performance comparison">
</p>

동일한 20/20/20 nm L-H-L geometry에서 Al, Ti, W, TiN, Mo 기반의 **10개 Low/High-WF 조합**을 비교했습니다. 구조·도핑·mesh·bias를 동일하게 유지하고 WF만 변화시켜 Ion, Ioff, DIBL, GIDL과 threshold 특성을 비교했습니다.

단일 지표의 최댓값 또는 최솟값만으로 조합을 선택하지 않고 drive current, leakage와 electrostatic control의 균형을 기준으로 후보를 좁혔습니다.

<table>
<tr>
<td width="55%"><img src="assets/figures/workfunction_pairs_idvg.webp" alt="Transfer characteristics of work-function pairs"></td>
<td width="45%"><img src="assets/figures/workfunction_cbe_comparison.webp" alt="Conduction band comparison of representative work-function pairs"></td>
</tr>
</table>

High = TiN인 대표 후보들을 대상으로 electric field, conduction-band energy와 BTBT generation을 추가 비교하여 최종적으로 다음 조합을 선정했습니다.

**Ti / TiN / Ti = 4.33 / 4.70 / 4.33 eV**

이 조합은 이후 모든 geometry 분석에서 고정된 material condition으로 사용했습니다.

---

## 형상 최적화 분석

<sub>Research record: 49-point Xbnd1–Xbnd2 exploration</sub>

<p align="center">
  <img src="assets/figures/geometry_49point_maps.webp" width="900" alt="49-point geometry performance maps">
</p>

Work-function 조합을 고정한 뒤 두 gate boundary인 `Xbnd1`, `Xbnd2`를 각각 2 nm 간격으로 변화시켜 **7 × 7 = 49개 geometry**를 비교했습니다.

세 metal length는 독립 변수가 아니라 다음 관계로 결정됩니다.

```text
M1 = Xbnd1 - 20
M2 = Xbnd2 - Xbnd1
M3 = 80 - Xbnd2
```

<p align="center">
  <img src="assets/figures/high_wf_length_trend.webp" width="760" alt="Performance trend versus center high-work-function length">
</p>

가장 뚜렷한 경향은 두 boundary의 절대 위치보다 **중앙 High-WF 구간 M2의 길이**에서 나타났습니다. M2가 짧아질수록 Ion은 완만하게 증가했지만, 일정 길이 이하에서는 Ioff와 DIBL이 빠르게 악화되었습니다.

이를 바탕으로 단일 최고 성능점이 아니라 여러 지표가 Single-Metal baseline 대비 함께 개선되는 대표 조건을 nominal로 선정했습니다.

**Nominal:** `Xbnd1 / Xbnd2 = 35 / 67 nm`  
**M1 / M2 / M3:** `15 / 32 / 13 nm`

---

## 변수화 검증 분석

<sub>Research record: geometry parameterization verification</sub>

<table>
<tr>
<td width="50%"><img src="assets/figures/parameterization_geometry.webp" alt="Parameterized gate geometry verification"></td>
<td width="50%"><img src="assets/figures/parameterization_mesh.webp" alt="Boundary-following mesh verification"></td>
</tr>
</table>

공정 변동 분석에서는 nominal 구조를 유지한 채 Xbnd1과 Xbnd2만 독립적으로 움직여야 합니다. 따라서 구조 생성 코드를 두 boundary 위치로 parameterize하고, boundary 변화 시 pillar와 oxide의 연속성, gate contact 분할과 local mesh refinement가 정상적으로 이동하는지 확인했습니다.

이 검증은 이후 tolerance sweep에서 계산된 전기적 변화가 geometry parameterization 오류나 고정 mesh 영역에서 발생한 artifact가 아닌지 확인하기 위한 단계입니다.

---

## 2D–3D 검증 분석

<p align="center">
  <img src="assets/figures/2d_3d_validation.webp" width="840" alt="2D axisymmetric and full-3D comparison">
</p>

대규모 geometry sweep에는 계산 효율을 위해 2D cylindrical axisymmetric model을 사용했습니다. 이 모델의 적용 범위를 확인하기 위해 Single-Metal과 nominal TLMG 구조를 full-3D로 추가 계산하고 같은 지표를 비교했습니다.

Nominal TLMG의 Single-Metal 대비 변화는 다음과 같습니다.

아래 표는 앞서 확정한 Single-Metal Gate baseline을 대조군으로 두고, TLMG VCAT 적용에 따른 성능 변화를 비교한 것입니다. 2D와 3D 결과는 서로 직접 비교한 값이 아니라, 각각의 Single-Metal Gate 대조군에 대해 TLMG VCAT이 보인 상대적인 성능 변화율을 정리한 것입니다.

| Metric | 2D | Full 3D |
|---|---:|---:|
| DIBL | −37.64% | −38.97% |
| Ion | +23.57% | +22.97% |
| Ioff | −70.40% | −65.83% |
| Ion/Ioff | +317.45% | +259.85% |
| GIDL | −87.53% | Not simulated |

Ion과 DIBL은 2D–3D 간 차이가 작았고, Ioff와 Ion/Ioff처럼 leakage에 민감한 지표에서는 더 큰 정량적 차이가 나타났습니다. 따라서 2D 모델은 geometry trend 탐색에 사용하되, leakage-derived metric에는 3D 보정 필요성을 남겼습니다.

---

## 민감도 분석

<sub>Research record: local boundary sensitivity</sub>

<p align="center">
  <img src="assets/figures/sensitivity_summary.webp" width="900" alt="Normalized sensitivity of Xbnd1 and Xbnd2">
</p>

Nominal 주변에서 Xbnd1과 Xbnd2의 국소 변화가 각 metric에 미치는 영향을 비교했습니다. 단순한 one-at-a-time response뿐 아니라 conditional slope와 boundary interaction을 함께 확인하여 두 경계의 영향이 독립적이지 않을 수 있음을 검토했습니다.

이 분석을 통해 tolerance study의 독립 변수를 Xbnd1과 Xbnd2로 유지하고, nominal 주변을 1 nm 단위로 세분화한 geometry sweep으로 확장했습니다.

---

## 공정 변동 분석

<sub>Research record: nominal-centered geometry variation</sub>

<p align="center">
  <img src="assets/figures/variation_46point_maps.webp" width="900" alt="Performance maps for 46 geometry variations">
</p>

Nominal `35/67 nm` 주변의 Xbnd1–Xbnd2 공간을 1 nm 단위로 세분화하여 총 **46개 geometry**를 평가했습니다. 각 geometry마다 Forward 0.05 V, Forward 1.0 V, GIDL 조건을 계산하여 총 **138회의 SDevice run**을 수행했습니다.

가장 일관된 악화 방향은 **high-Xbnd1 / low-Xbnd2**, 즉 중앙 High-WF 구간 M2가 짧아지는 방향이었습니다. 이 방향에서 DIBL과 Ioff가 증가하고 Ion/Ioff가 감소하여, nominal 주변의 설계 margin이 비대칭적으로 형성됨을 확인했습니다.

> 본 분석은 실제 wafer 통계나 공정 수율을 직접 측정한 결과가 아니라, deterministic geometry grid를 이용한 device-level variation study입니다.

---

## 제안

<table>
<tr>
<td width="50%"><img src="assets/figures/tolerance_performance_plane.webp" alt="DIBL versus Ion/Ioff tolerance bands"></td>
<td width="50%"><img src="assets/figures/tolerance_geometry_map.webp" alt="Tolerance band mapped to geometry space"></td>
</tr>
</table>

Tolerance window는 새로운 최적점을 찾기 위한 기준이 아니라, 선정된 nominal과 비교해 어느 범위까지 유사한 electrical performance가 유지되는지를 평가하기 위해 정의했습니다.

주 평면은 `DIBL – Ion/Ioff`이며 nominal 대비 양방향 편차를 적용했습니다.

- **Core:** DIBL과 Ion/Ioff 모두 nominal 대비 ±10%
- **Outer:** DIBL과 Ion/Ioff 모두 nominal 대비 ±20%
- **Guard:** Ion ≥ Single-Metal, Ioff ≤ Single-Metal, GIDL ≤ Single-Metal
- **Monitor:** SS < 75 mV/dec

46개 geometry 중 **18개가 Core**, **29개가 Outer-inclusive** 조건을 만족했습니다. Outer-inclusive 비율은 46개 중 **63.0%**입니다.

형상 공간에서는 단순한 min–max 범위가 아니라, 내부의 모든 조합이 실제 계산되고 통과한 nominal 포함 최대 사각형인 **observed-grid rectangle**을 대표 tolerance window로 사용했습니다.

| Window | Xbnd1 | Xbnd2 |
|---|---:|---:|
| Core | 33–35 nm | 65–67 nm |
| Outer | 33–36 nm | 65–67 nm |

이 범위를 **L-H-L Gate Segmentation Geometry Tolerance Window**로 정의했습니다.

---

## 누설 강건성 분석

<p align="center">
  <img src="assets/figures/gidl_robustness.webp" width="780" alt="GIDL distribution around the nominal geometry">
</p>

Tolerance classification은 DIBL과 Ion/Ioff를 중심으로 구성했지만, DIBL 안정성이 BTBT leakage 안정성을 자동으로 의미하지 않기 때문에 GIDL을 별도의 guard metric으로 확인했습니다.

46개 geometry의 GIDL은 약 `3.583×10⁻¹⁵–3.627×10⁻¹⁵ A`의 좁은 범위에 분포했고, 가장 불리한 형상에서도 Single-Metal baseline보다 **87.40% 이상 낮게 유지**되었습니다.

따라서 본 sweep 범위에서는 GIDL보다 DIBL과 Ion/Ioff가 tolerance window를 제한하는 주요 지표로 작용했습니다.

---

## 결과

최종 nominal Three-Layer Metal-Gate VCAT은 Single-Metal baseline과 비교하여 drive current와 leakage/electrostatic metric을 동시에 개선했습니다.

### Nominal 구조 분석

```text
Ti / TiN / Ti
4.33 / 4.70 / 4.33 eV

M1 / M2 / M3
15 / 32 / 13 nm

Xbnd1 / Xbnd2
35 / 67 nm
```

### 주요 결과 분석

- 2D DIBL: **37.64% 감소**
- 2D Ion: **23.57% 증가**
- 2D Ioff: **70.40% 감소**
- 2D Ion/Ioff: **317.45% 증가**
- 2D GIDL: **87.53% 감소**
- 3D DIBL: **38.97% 감소**
- 3D Ion/Ioff: **259.85% 증가**
- 46-geometry tolerance sweep: **138 SDevice runs**
- Outer-inclusive tolerance band: **29 / 46 geometries, 63.0%**

본 연구에서 nominal은 49-point sweep의 절대 최고점이 아니라, Single-Metal 대비 주요 지표가 함께 개선되고 이후 variation study의 기준으로 사용할 수 있는 **balanced reference geometry**입니다.

---

## 한계

본 결과의 적용 범위는 다음과 같이 제한됩니다.

- Tolerance window는 **2D axisymmetric model** 기반입니다.
- Geometry variation은 확률 분포가 아닌 **deterministic grid sweep**입니다.
- Core ±10% / Outer ±20%는 산업 표준 공정 수율 기준이 아니라 nominal 대비 electrical deviation 기준입니다.
- GIDL은 mesh에 민감하므로 절대값보다 동일 조건 내 상대 비교와 guard 판단에 사용했습니다.
- Gate-oxide tunneling과 interface trap이 포함되지 않아 Ioff와 Ion/Ioff는 모델 범위 내 상대 비교 지표입니다.
- 일부 Outer boundary는 미실행 holdout geometry 때문에 추가 검증 여지가 남아 있습니다.
- Full-3D tolerance sweep과 statistical process variation은 후속 연구 대상입니다.

---

## 공동 연구 기록

본 프로젝트는 숭실대학교 학생 5인이 공동 수행한 연구이며, 최종 보고서에서는 모든 저자가 동등 기여로 표기되었습니다. 이 포트폴리오는 공동 연구 전체를 개인 연구로 재표현하지 않고, 연구 흐름과 공개 가능한 핵심 결과를 포트폴리오 형식으로 재구성한 것입니다.

세부 Phase 기록, 작업 이력과 공용 연구 자료는 공동 연구 저장소에서 확인할 수 있습니다.

- [Collaborative Research Repository](https://github.com/jujushmaterial/VCAT-1T1C-DRAM-TCAD-Research)
- [Ju Sanghyeon Research Records](https://github.com/jujushmaterial/VCAT-1T1C-DRAM-TCAD-Research/tree/main/members/JuSanghyeon)

---

## 발표 결과

<p align="center">
  <img src="assets/poster/final_poster.webp" width="760" alt="2026 Next-Generation Semiconductor Competition poster">
</p>

본 연구는 2026 차세대반도체 경진대회에서 포스터 형식으로 발표되었습니다. 포스터는 연구 배경, 단계적 검증 workflow, nominal 구조 선정, 2D–3D 비교, tolerance window와 최종 설계 시사점을 요약합니다.

발표용 PPT와 발표 대본은 연구 내용 정리와 포트폴리오 구성의 참고 자료로만 사용하며 본 저장소에는 공개하지 않습니다.

---

## 참고 문헌

1. S.-Y. Lee, K.-N. Park, S. Kim, and J.-K. Han, “Three-layer work-function gate for suppressing floating-body effects on vertical-channel DRAM access transistors,” *Applied Physics Letters*, vol. 128, 123304, 2026.
2. D. Kong, H. Lee, J.-H. Lee, and J. Jeon, “Location-Selective Dual Work-Function Engineering for DRAM Vertical Cell Transistors,” *IEEE Electron Device Letters*, accepted 2026.

세부 참고문헌과 연구별 인용 관계는 최종 연구 보고서 및 공동 연구 저장소를 기준으로 관리합니다.
