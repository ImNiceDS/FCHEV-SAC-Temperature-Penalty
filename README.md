# FCHEV Energy Management with SAC-Beta & Temperature-Penalty Reward  
수소전기차(FCHEV) 강화학습 기반 에너지 관리 최적화 연구

---

## 📘 Overview

본 저장소는 기존 논문  
**"Health-considered Energy Management Strategy for FCHEV using Improved SAC with Beta Policy"**  
의 구현을 재현하고, 추가로 **배터리 코어 온도 기반 패널티(Temperature Penalty)** 를 적용한 새로운 보상 구조로 확장한 연구를 포함한다.

목표는 열적 안전성 향상, SOC 안정화, 보다 현실적인 EMS 제어 정책을 학습하는 것이다.

### 본 프로젝트는 다음을 포함한다:
- SAC-Beta 기반 EMS 원본 논문 코드 재현  
- **SOC + 배터리 온도 패널티 기반 Reward 설계**  
- Train / Valid 주행 사이클 실험 비교  
- Jupyter Notebook 분석 및 최종 발표 자료(PPT)

---

## 📄 Abstract (Updated for This Research)

본 연구는 SAC-Beta 기반 Fuel Cell Hybrid Electric Vehicle(FCHEV) 에너지 관리 전략을 재현하고 확장한 프로젝트이다.  
기존 EMS는 SOC 유지 및 수소 소비 최적화는 우수하지만, **배터리 열적 안전성(thermal safety)** 을 고려하지 않아 고부하 상황에서 과열 위험이 존재한다.

이를 해결하기 위해 본 연구에서는 **배터리 코어 온도 58°C 초과 시 Temperature Penalty** 를 부과하는 새로운 보상 구조를 설계하였다.

### 실험 결과 요약:
- 최대 배터리 온도 감소 → 열적 안정성 향상  
- SOC가 안정적으로 30% 이상 유지됨  
- 더욱 현실적인 EMS 제어 정책 학습  
- 수소 소비는 소폭 증가하나 안전성 측면의 개선이 더 큼  

---

## 📁 Project Structure

```
FCHEV-SAC-Temperature-Penalty/
├── README.md                        # 프로젝트 문서
├── main.ipynb                       # 메인 학습/실행 노트북
├── Beta_test.ipynb                  # Beta 분포 테스트 노트북
├── dp_main_new.ipynb                # Dynamic Programming 비교 실험
├── dqn_main.ipynb                   # DQN 비교 실험
├── powerdemand.ipynb                # 전력 수요 분석
│
├── common/                          # 공통 모듈
│   ├── arguments.py                 # 실험 설정 및 하이퍼파라미터
│   ├── runner.py                    # 학습 루프 및 실행 관리
│   ├── sac.py                       # SAC-Beta 알고리즘 구현
│   ├── network.py                   # Actor/Critic 신경망 정의
│   ├── agentEMS.py                  # EMS 에이전트 래퍼
│   ├── env.py                       # FCHEV 환경 정의
│   ├── evaluate.py                  # 모델 평가 스크립트
│   ├── memory.py                    # Replay Buffer
│   ├── utils.py                     # 유틸리티 함수
│   ├── FCHEV_SOH.py                 # Fuel Cell & Battery SOH 모델
│   ├── Cell.py                      # 배터리 셀 모델링
│   ├── ddpg.py                      # DDPG 알고리즘 (비교용)
│   ├── dqn_model.py                 # DQN 알고리즘 (비교용)
│   └── DP_EMS_agent.py              # DP 에이전트 (비교용)
│
├── eva_SAC_CS_Beta/                 # Evaluation 스크립트
│   └── root/
│
├── test5_SAC_CS_Beta/               # 원본 논문 재현 (SOC 기반 Reward)
│   └── MixTrain_w100_LR1e-03_v1/
│       └── episode_data/            # 학습된 모델 체크포인트
│
├── test8_SAC_CS_Beta/               # 논문 재현 + 배터리 온도 로깅
│   └── MixTrain_w100_LR1e-03_v1_86/
│       └── episode_data/
│
├── test9_SAC_CS_Beta/               # 개선된 Reward (SOC + Temp Penalty)
│   └── MixTrain_w100_LR1e-03_v1/
│       └── episode_data/
│
├── logs5_SAC_CS_Beta/               # 학습 로그 저장
│
└── project-data-main/               # 데이터셋
    ├── standard_driving_cycles/     # 주행 사이클 데이터
    ├── FCHEV_data/                  # FCHEV 컴포넌트 파라미터
    └── HEV_data/                    # HEV 비교 데이터
```

---

## 🚀 How to Run

### 1) Configuration File
모든 실험 설정은 아래 파일에서 제어된다:

`common/arguments.py`

주요 옵션:
- `--mode`: train / eval  
- `--scenario_name`: CLTC-P / WVU-INTER / MixTrain  
- `--reward_mode`: original / temp_penalty  
- `--seed`: reproducibility 설정  
- `--max_episodes`: 학습 에피소드 수  

---

### 2) Training 실행

```bash
python main.py --mode train --scenario_name MixTrain --reward_mode temp_penalty
```

### 3) Evaluation

```bash
python main.py --mode eval --scenario_name MixValid --model_path <saved_model_directory>
```

---

## Trained Models (Deep Learning Checkpoints)

### 📁 test5_SAC_CS_Beta — Original Reward (논문 재현)

```
test5_SAC_CS_Beta/MixTrain_w100_LR1e-03_v1/episode_data
```

### 📁 test8_SAC_CS_Beta — Original Reward + Battery Temperature Logging

```
test8_SAC_CS_Beta/MixTrain_w100_LR1e-03_v1_86/episode_data
```

### 📁 test9_SAC_CS_Beta — Improved Reward (SOC + Temperature Penalty)

```
test9_SAC_CS_Beta/MixTrain_w100_LR1e-03_v1_73/episode_data
```

---

## Report (PPT)

프로젝트 발표 자료(PPT)는 저장소 내 포함됨.

---

## Data Source

1. **Driving Cycles**  
   🔗 https://github.com/ImNiceDS/FCHEV-SAC-Temperature-Penalty/tree/main/project-data-main/standard_driving_cycles

2. **FCHEV Power System Data**  
   🔗 https://github.com/ImNiceDS/FCHEV-SAC-Temperature-Penalty/tree/main/project-data-main/FCHEV_data

---

## Citation

```bibtex
@article{chen2023health,
  title={Health-considered energy management strategy for fuel cell hybrid electric vehicle based on improved soft actor critic algorithm adopted with Beta policy},
  author={Chen, Weiqi and Peng, Jiankun and Chen, Jun and Zhou, Jiaxuan and Wei, Zhongbao and Ma, Chunye},
  journal={Energy Conversion and Management},
  volume={292},
  pages={117362},
  year={2023},
  publisher={Elsevier}
}
```

> 본 프로젝트는 기존 연구의 환경 모델을 베이스로 하되, 보상 함수(Reward Function)를 재설계 및 추가하여 개선한 프로젝트입니다.

