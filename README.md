# Conditional-DCGAN 기반 감정 음성 데이터 증강  

---

## 📌 **프로젝트 개요**  

- **주제:** Conditional-DCGAN 기반 감정 음성 데이터 증강을 통한 음성 감정 인식 성능 향상  
- **목적:** 소량의 감정 음성 데이터 문제 해결 및 음성 감정 인식 모델 성능 최적화  
- **모델:** Conditional-DCGAN 및 CNN+Bi-LSTM 모델 적용  
- **성과:** 증강 데이터로 **WA 91.46%, UAR 91.61%** 달성  
- **논문 출판:**  
  - 본 프로젝트의 성과는 MDPI *Applied Sciences* 저널에 논문으로 게재됨.  
  - **제목:** "Enhanced Speech Emotion Recognition Using
 Conditional-DCGAN-Based Data Augmentation"  
  - **DOI:** [10.3390/app14219890](https://www.mdpi.com/2076-3417/14/21/9890)  


---

## 📌 **문제 정의 및 도전 과제**  

- **소량의 데이터 문제:**  
  - 음성 감정 인식 데이터셋이 제한적이어서 모델 일반화 성능 저하  

- **전통적인 DCGAN의 한계:**  
  - 초기 학습 불안정성: 적은 데이터로 판별기의 성능이 낮아 생성기 학습 성능 저하  
  - 감정 정보 반영 부족: 생성된 데이터가 감정을 잘 반영하는지 직접 평가 불가  

- **특징 추출의 한계:**  
  - 기존 특징 추출 방법(MFCC, Spectrogram)의 감정 표현 부족  

---

## 📌 **모델 설계 및 구현**  

### **1. Conditional-DCGAN 기반 데이터 증강**  

- **입력 데이터:**  
  - 음성 데이터를 **Mel-spectrogram**으로 변환하여 입력  
  - 감정 레이블(Conditional Label)을 추가하여 생성기 학습 강화  

- **증강 전략:**  
  - Conditional-DCGAN을 활용해 다양한 감정을 반영한 Mel-spectrogram 생성  
  - 데이터 다양성 확보 및 감정 정보 보존  

---

### **2. 손실 최적화 및 강화학습 적용**  

#### **1) 기존 DCGAN의 문제점:**  
- 생성기와 판별기 간의 불균형 학습 문제  
- 감정 표현을 직접 평가할 수 없는 손실 함수 구조  

#### **2) 새로운 학습 전략 제안:**  
- **감정 인식 모델 추가:** Conditional-DCGAN에 **감정 인식 모델** 결합  
- **보상 기반 손실 함수 도입:** 감정 인식 결과를 보상(Reward)으로 활용  

![equation](https://latex.codecogs.com/svg.latex?\mathcal{L}_{total}=\mathcal{L}_{GAN}+\lambda\cdot\mathcal{L}_{emotion})



- **손실 함수 설명:**  
    - ![equation](https://latex.codecogs.com/svg.latex?\mathcal{L}_{GAN}) : 기존 GAN 손실(Binary Cross Entropy) 적용  
    - ![equation](https://latex.codecogs.com/svg.latex?\mathcal{L}_{emotion}) : 감정 인식 확률 기반 보상 신호  
    - ![equation](https://latex.codecogs.com/svg.latex?\lambda) : 두 손실 항의 가중치 조절  


#### **3) 강화학습 개념 도입:**  
- 감정을 잘 표현하는 Mel-spectrogram에 높은 보상을 부여하여 학습 안정성 강화  
- 학습 초기 불균형 완화 및 데이터 소량 문제 극복  

---

### **3. CNN+Bi-LSTM 기반 감정 인식 모델**  

- **특징 학습 구조:**  
  - **CNN:** Mel-spectrogram에서 공간적 특징 추출  
  - **Bi-LSTM:** 시간적 감정 변화 학습  

- **최적화 기법 적용:**  
  - **AdamW 옵티마이저:** 가중치 최적화 및 학습 안정성 강화  
  - **Dropout 및 Batch Normalization:** 과적합 방지 및 성능 향상  
---

## 📌 **데이터 처리 및 증강**  
1. **사용한 데이터셋**
    - **EmoDB**

2. **전처리 과정:**  
   - **Librosa 활용:** 잡음 제거, 정규화, 샘플링 주파수 설정(16kHz)  
   - **특징 추출:** Mel-spectrogram 생성  

3. **데이터 증강 과정:**  
   - Conditional-DCGAN을 활용하여 새로운 Mel-spectrogram 생성  
   - 감정 정보가 보존되도록 강화학습 기반 최적화 적용  

4. **평가 및 검증:**  
   - 증강된 데이터의 감정 표현 검증을 위한 CNN+Bi-LSTM 모델 학습 및 테스트  

---

## 📌 **학습 과정 및 결과 분석**  

### **훈련 및 검증 프로세스:**  

1. **Conditional-DCGAN 훈련**  
   - **판별기 및 생성기 초기 학습 안정화 적용**  
   - 감정 인식 모델을 활용한 보상 신호 기반 학습 진행  

2. **감정 인식 모델 학습**  
   - CNN+Bi-LSTM을 활용한 성능 최적화  
   - 증강 데이터 포함 시 성능 향상 확인  

---

### **성능 지표:**  

- **기존 데이터:**  
  - **WA:** 79.31%, **UAR:** 78.16%  

- **증강 데이터 포함:**  
  - **WA:** 91.46%, **UAR:** 91.61%  

---

### **성과 분석:**  
- **감정 인식 성능 향상:** 최대 7% 이상의 성능 향상  
- **데이터 증강 효과 검증:** 강화학습 기반 손실 최적화의 성능 향상 기여  
- **학습 안정성 확보:** 적은 데이터에도 강건한 학습 성능 달성  

---

## 📌 **결론 및 기여**  

- **데이터 증강 기법 제안:** Conditional-DCGAN과 감정 인식 모델을 결합한 새로운 손실 함수 구성  
- **성능 최적화:** 기존 DCGAN 대비 학습 안정성과 감정 표현 최적화 성능 향상  
- **확장 가능성:** 적은 데이터에서도 다양한 감정 표현을 생성할 수 있는 모델 구조 제안  

---

## 📌 **사용 기술 및 도구**  

- **프로그래밍 언어:** Python  
- **딥러닝 프레임워크:** PyTorch, TensorFlow  
- **최적화 기법:** AdamW, Dropout, Batch Normalization

---
