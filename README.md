## Overview

This repository reproduces the original **“Health-considered energy management strategy for FCHEV using improved SAC with Beta Policy”**  
and extends it by redesigning the reward with a **battery core temperature penalty** to improve thermal safety, SOC stability,  
and realistic EMS behavior.

The project includes:
- Reproduction of the original SAC-Beta EMS  
- Reward redesign using **SOC + Battery Temperature Penalty**  
- Comparison experiments across Train / Valid driving cycles  
- Jupyter notebook analysis and final PPT report  

---

## Abstract (Updated for This Research)

This project implements and extends the SAC-Beta Energy Management Strategy (EMS) for Fuel Cell Hybrid Electric Vehicles (FCHEVs).  
The original SAC-Beta EMS effectively stabilizes SOC and optimizes hydrogen consumption but does not explicitly consider  
battery thermal safety, making the system vulnerable to overheating during high-power driving.

To address this limitation, we redesign the reward by introducing a **temperature penalty** when battery core temperature exceeds 58°C.  
This improves thermal safety and encourages the agent to distribute power more conservatively between the fuel cell system (FCS)  
and the battery.

Experimental results show that the redesigned reward:
- significantly reduces peak battery temperatures,  
- maintains SOC above 30%,  
- improves thermal stability across both Train and Valid cycles,  
- results in more realistic EMS behavior,  
- with only a moderate increase in hydrogen consumption.

---
## Project Structure
```
common/                     # 환경 설정, arguments, utility functions
eva_SAC_CS_Beta/            # evaluation scripts
test5_SAC_CS_Beta/          # 원본 논문 재현 (SOC 기반 reward)
test8_SAC_CS_Beta/          # 논문 재현 + 배터리 코어 온도 로그 저장
test9_SAC_CS_Beta/          # 보상 조건 개선 (SOC + Temperature penalty)
project-data-main/          # driving cycles, FCHEV component data
*.ipynb                     # 분석 및 결과 플로팅 노트북
README.md
```


## How to Run

### 1) Configuration File

모든 실험 설정은 아래 파일에서 제어:
common/arguments.py

주요 옵션:
- `--mode`: train / eval  
- `--scenario_name`: CLTC-P / WVU-INTER / MixTrain  
- `--reward_mode`: original / temp_penalty  
- `--seed`: reproducibility 설정  
- `--max_episodes`: 학습 에피소드 수  

---

### 2) Training

```
python main.py --mode train --scenario_name MixTrain --reward_mode temp_penalty
```

### 3) Evaluation

```
python main.py --mode eval --scenario_name MixTrain --model_path <saved_model_directory>
```

---

## Trained Models (Deep Learning Checkpoints)

### 📁 test5_SAC_CS_Beta — Original Reward (논문 재현)

test5_SAC_CS_Beta/MixTrain_w100_LR1e-03_v1/episode_data


### 📁 test8_SAC_CS_Beta — Original Reward + Battery Temperature Logging

test8_SAC_CS_Beta/MixTrain_w100_LR1e-03_v1_86/episode_data


### 📁 test9_SAC_CS_Beta — Improved Reward (SOC + Temperature Penalty)

test9_SAC_CS_Beta/MixTrain_w100_LR1e-03_v1_73/episode_data

---

## Report (PPT)

프로젝트 발표 자료(PPT)는 저장소 내 포함됨:

---

## Data Source

## Driving Cycles
🔗 https://github.com/ImNiceDS/FCHEV-SAC-Temperature-Penalty/tree/8594e030dc4df073a110a6628f525bcd52db4c65/project-data-main/standard_driving_cycles

## FCHEV Power System Data
🔗 https://github.com/ImNiceDS/FCHEV-SAC-Temperature-Penalty/tree/8594e030dc4df073a110a6628f525bcd52db4c65/project-data-main/FCHEV_data


---

## Citation

```
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


