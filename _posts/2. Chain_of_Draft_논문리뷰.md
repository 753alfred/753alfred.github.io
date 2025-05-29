# Chain-of-Draft: 효율적인 추론을 위한 간결한 사고 체인

**논문 출처**: [arXiv:2502.18600](https://arxiv.org/pdf/2502.18600v2)

**저자**: Silei Xu†, Wenhao Xie, Lingxiao Zhao, Pengcheng He

**소속**: Zoom Communications

## Abstract

- Chain-of-Draft (CoD)는 대규모 언어 모델에서 추론 효율성과 정확도를 동시에 개선하는 새로운 프롬프팅 전략임
- 기존 Chain-of-Thought (CoT) 방법의 높은 토큰 사용량과 추론 지연 문제를 해결하기 위해 개발됨
- 각 추론 단계를 최대 5단어로 제한하여 간결한 초안(draft) 형태로 사고 과정을 압축
- GPT-4o와 Claude 3.5 Sonnet에서 CoT 대비 80% 이상의 토큰 절약을 달성하면서도 90% 이상의 정확도를 유지
- 산술 추론, 상식 추론, 기호적 추론 등 다양한 영역에서 효과성을 입증
- Few-shot 학습 환경에서 최적의 성능을 발휘하며, 대규모 모델에서 특히 효과적임

## 1. Introduction

- Chain-of-Draft (CoD)는 대규모 언어 모델의 추론 효율성을 향상시키기 위한 새로운 프롬프팅 기법임
- 기존의 Chain-of-Thought (CoT) 방법이 뛰어난 정확도를 보이지만 많은 토큰을 사용하여 추론 지연을 발생시키는 문제를 해결하고자 함

![Figure 1: Comparison of Claude 3.5 Sonnet's accuracy and token usage](https://arxiv.org/html/2502.18600v2/extracted/6244873/plot.png)

**Figure 1**: Claude 3.5 Sonnet의 정확도와 토큰 사용량 비교. 세 가지 프롬프트 전략에서 CoD는 CoT와 유사한 정확도를 달성하면서도 훨씬 적은 토큰을 사용함

- CoD의 핵심 아이디어는 각 추론 단계를 최대 5단어로 제한하여 간결한 초안(draft) 형태로 사고 과정을 표현하는 것임
- 이를 통해 추론의 핵심 요소만을 유지하면서도 계산 효율성을 크게 향상시킬 수 있음

## 2. Related Work

### 2.1 Structured Reasoning Frameworks for LLMs

- 대규모 언어 모델을 위한 구조화된 추론 프레임워크는 모델의 문제 해결 능력을 체계적으로 향상시키기 위한 연구 분야임
- Chain-of-Thought 방법론이 대표적인 예시로, 단계별 추론 과정을 명시적으로 표현하여 복잡한 문제에 대한 모델의 성능을 크게 개선함

### 2.2 LLM Inference Latency Reduction

- LLM 추론 지연 시간 감소는 실용적인 AI 시스템 구축에 있어 핵심적인 과제임
- 기존 연구들은 모델 압축, 토큰 효율성 개선, 추론 최적화 등 다양한 접근 방식을 통해 이 문제를 해결하고자 함

## 3. Chain-of-Draft Prompting

### 3.1 방법론 개요

- Chain-of-Draft는 기존 CoT의 장점을 유지하면서도 토큰 효율성을 크게 개선한 프롬프팅 기법임
- 핵심은 각 사고 단계를 최대 5단어로 제한하여 간결한 초안 형태로 표현하는 것임

### 3.2 프롬프팅 전략 비교

**Standard 방식:**
```plaintext
Q: Jason had 20 lollipops. He gave Denny some lollipops. Now Jason has 12 lollipops. How many lollipops did Jason give to Denny?
A: 8 
```
- **질문**: Jason은 사탕 20개를 가지고 있었습니다. 그는 Denny에게 사탕을 몇 개 주었습니다. 이제 Jason은 사탕 12개를 가지고 있습니다. Jason이 Denny에게 몇 개의 사탕을 주었나요?
- **답**: 8

**Chain-of-Thought 방식:**
```plaintext
Q: Jason had 20 lollipops. He gave Denny some lollipops. Now Jason has 12 lollipops. How many lollipops did Jason give to Denny?
A: Let's think through this step by step: 
    1. Initially, Jason had 20 lollipops.
    2. After giving some to Denny, Jason now has 12 lollipops.
    3. To find out how many lollipops Jason gave to Denny, we need to calculate the difference between the initial number of lollipops and the remaining number.
    4. We can set up a simple subtraction problem: Initial number of lollipops - Remaining number of lollipops = Lollipops given to Denny
    5. Putting in the numbers: 20 - 12 = Lollipops given to Denny
    6. Solving the subtraction: 20 - 12 = 8 Therefore, Jason gave 8 lollipops to Denny
```
- **질문**: Jason은 사탕 20개를 가지고 있었습니다. 그는 Denny에게 사탕을 몇 개 주었습니다. 이제 Jason은 사탕 12개를 가지고 있습니다. Jason이 Denny에게 몇 개의 사탕을 주었나요?
- **답**: 이것을 단계별로 생각해봅시다:  
  1. 처음에 Jason은 사탕 20개를 가지고 있었습니다.  
  2. Denny에게 몇 개를 준 후, Jason은 이제 사탕 12개를 가지고 있습니다.  
  3. Jason이 Denny에게 몇 개의 사탕을 주었는지 알아내기 위해, 초기 사탕 수와 남은 사탕 수의 차이를 계산해야 합니다.  
  4. 간단한 뺄셈 문제를 설정할 수 있습니다: 초기 사탕 수 - 남은 사탕 수 = Denny에게 준 사탕 수.  
  5. 숫자를 대입하면: 20 - 12 = Denny에게 준 사탕 수.  
  6. 뺄셈을 풀면: 20 - 12 = 8. 따라서 Jason은 Denny에게 사탕 8개를 주었습니다.

**Chain-of-Draft 방식:**
```plaintext
Q: Jason had 20 lollipops. He gave Denny some lollipops. Now Jason has 12 lollipops. How many lollipops did Jason give to Denny?
A: 20 - x = 12; x = 20 - 12 = 8. #### 8
```
- **질문**: Jason은 사탕 20개를 가지고 있었습니다. 그는 Denny에게 사탕을 몇 개 주었습니다. 이제 Jason은 사탕 12개를 가지고 있습니다. Jason이 Denny에게 몇 개의 사탕을 주었나요?
- **답**: 20 - x = 12; x = 20 - 12 = 8. #### 8

## 4. Experiments

### 4.1 Experimental Setup

실험에서는 세 가지 프롬프팅 전략을 비교함:

**Standard 프롬프트:**
```plaintext
Answer the question directly. Do not return any preamble, explanation, or reasoning.
```
- 질문에 직접 답변하세요. 서문, 설명, 또는 추론을 반환하지 마세요.

**Chain-of-Thought 프롬프트:**
```plaintext
Think step by step to answer the following question. Return the answer at the end of the response after a separator ####.
```
- 다음 질문에 답하기 위해 단계별로 생각하세요. 구분자 #### 뒤에 응답 끝에 답을 반환하세요.

**Chain-of-Draft 프롬프트:**
```plaintext
Think step by step, but only keep a minimum draft for each thinking step, with 5 words at most. Return the answer at the end of the response after a separator ####.
```
- 단계별로 생각하되, 각 생각 단계마다 최대 5단어로 최소한의 초안만 유지하세요. 구분자 #### 뒤에 응답 끝에 답을 반환하세요.

### 4.2 Arithmetic Reasoning

GSM8K 데이터셋을 사용한 산술 추론 평가 결과:

| Model | Prompt | Accuracy | Token # | Latency |
|-------|--------|----------|---------|---------|
| GPT-4o | Standard | 53.3% | 1.1 | 0.6 s |
| GPT-4o | CoT | 95.4% | 205.1 | 4.2 s |
| GPT-4o | CoD | 91.1% | 43.9 | 1.0 s |
| Claude 3.5 Sonnet | Standard | 64.6% | 1.1 | 0.9 s |
| Claude 3.5 Sonnet | CoT | 95.8% | 190.0 | 3.1 s |
| Claude 3.5 Sonnet | CoD | 91.4% | 39.8 | 1.6 s |

**Table 1**: GSM8K 평가 결과. CoD는 CoT 대비 80% 이상의 토큰 절약을 달성하면서도 90% 이상의 정확도를 유지함

### 4.3 Commonsense Reasoning

상식 추론 평가에서도 CoD는 우수한 성능을 보임:

**Date Understanding 결과:**
| Model | Prompt | Accuracy | Token # | Latency |
|-------|--------|----------|---------|---------|
| GPT-4o | Standard | 72.6% | 5.2 | 0.6 s |
| GPT-4o | CoT | 90.2% | 75.7 | 1.7 s |
| GPT-4o | CoD | 88.1% | 30.2 | 1.3 s |
| Claude 3.5 Sonnet | Standard | 84.3% | 5.2 | 1.0 s |
| Claude 3.5 Sonnet | CoT | 87.0% | 172.5 | 3.2 s |
| Claude 3.5 Sonnet | CoD | 89.7% | 31.3 | 1.4 s |

**Table 2**: Date understanding 평가 결과

**Sports Understanding 결과:**
| Model | Prompt | Accuracy | Token # | Latency |
|-------|--------|----------|---------|---------|
| GPT-4o | Standard | 90.0% | 1.0 | 0.4 s |
| GPT-4o | CoT | 95.9% | 28.7 | 0.9 s |
| GPT-4o | CoD | 98.3% | 15.0 | 0.7 s |
| Claude 3.5 Sonnet | Standard | 90.6% | 1.0 | 0.9 s |
| Claude 3.5 Sonnet | CoT | 93.2% | 189.4 | 3.6 s |
| Claude 3.5 Sonnet | CoD | 97.3% | 14.3 | 1.0 s |

**Table 3**: Sports understanding 평가 결과

### 4.4 Symbolic Reasoning

동전 뒤집기 문제를 통한 기호적 추론 평가:

```plaintext
Q: A coin is heads up. Robyn flips the coin. Peggy flips the coin. Grant flips the coin. Vanessa does not flip the coin. Is the coin still heads up?
A: No
```
- **질문**: 동전이 앞면입니다. Robyn이 동전을 뒤집습니다. Peggy가 동전을 뒤집습니다. Grant가 동전을 뒤집습니다. Vanessa는 동전을 뒤집지 않습니다. 동전이 여전히 앞면인가요?
- **답**: 아니오

| Model | Prompt | Accuracy | Token # | Latency |
|-------|--------|----------|---------|---------|
| GPT-4o | Standard | 73.2% | 1.0 | 0.4 s |
| GPT-4o | CoT | 100.0% | 52.4 | 1.4 s |
| GPT-4o | CoD | 100.0% | 16.8 | 0.8 s |
| Claude 3.5 Sonnet | Standard | 85.2% | 1.0 | 1.2 s |
| Claude 3.5 Sonnet | CoT | 100.0% | 135.3 | 3.1 s |
| Claude 3.5 Sonnet | CoD | 100.0% | 18.9 | 1.6 s |

**Table 4**: Coin flip 평가 결과

### 4.5 Limitations of CoD

#### 4.5.1 Inconsistency Without Few-shot Examples

Zero-shot 설정에서 CoD의 성능은 상당히 감소함:

| Model | Prompt | Accuracy | Token # | Latency |
|-------|--------|----------|---------|---------|
| GPT-4o | Standard | 56.9% | 2.2 | 0.5 s |
| GPT-4o | CoT | 94.8% | 278.4 | 8.1 s |
| GPT-4o | CoD | 84.4% | 76.4 | 2.6 s |
| Claude 3.5 Sonnet | Standard | 61.9% | 5.2 | 0.9 s |
| Claude 3.5 Sonnet | CoT | 90.4% | 248.8 | 3.5 s |
| Claude 3.5 Sonnet | CoD | 65.5% | 73.7 | 1.6 s |

**Table 5**: Zero-shot GSM8K 평가 결과

#### 4.5.2 Reduced Performance on Small Models

소규모 모델에서는 CoD의 효과가 제한적임:

| Model | Prompt | Accuracy | Token # |
|-------|--------|----------|---------|
| Qwen2.5-1.5B-Instruct | Standard | 5.7% | 6.6 |
| Qwen2.5-1.5B-Instruct | CoT | 32.5% | 141.4 |
| Qwen2.5-1.5B-Instruct | CoD | 24.2% | 75.1 |
| Qwen2.5-3B-Instruct | Standard | 7.2% | 3.4 |
| Qwen2.5-3B-Instruct | CoT | 59.1% | 236.4 |
| Qwen2.5-3B-Instruct | CoD | 43.1% | 41.2 |
| Llama3.2-3B-Instruct | Standard | 3.9% | 16.6 |
| Llama3.2-3B-Instruct | CoT | 70.7% | 195.3 |
| Llama3.2-3B-Instruct | CoD | 52.5% | 98.1 |
| Zoom-SLM-2.3B | Standard | 5.9% | 3.8 |
| Zoom-SLM-2.3B | CoT | 77.7% | 129.0 |
| Zoom-SLM-2.3B | CoD | 50.9% | 55.6 |

**Table 6**: 소규모 언어 모델에서의 GSM8K 평가 결과

## 5. Discussion

- Chain-of-Draft는 대규모 언어 모델의 추론 효율성을 크게 향상시키는 혁신적인 방법론임
- 주요 장점과 한계를 다음과 같이 정리할 수 있음

### 5.1 주요 성과

- **토큰 효율성**: CoT 대비 평균 80% 이상의 토큰 사용량 감소
- **정확도 유지**: 대부분의 태스크에서 CoT와 유사한 성능 달성
- **지연 시간 감소**: 추론 속도 크게 개선
- **다양한 태스크 적용 가능**: 산술, 상식, 기호적 추론 모두에서 효과적

### 5.2 한계점

- **Few-shot 의존성**: Zero-shot 환경에서 성능 저하
- **소규모 모델 제한**: 작은 모델에서는 효과가 제한적
- **태스크별 편차**: 일부 태스크에서는 CoT 대비 성능 차이 존재

### 5.3 향후 연구 방향

- Zero-shot 성능 향상을 위한 프롬프트 최적화
- 소규모 모델에 적합한 CoD 변형 개발
- 다양한 도메인에서의 효과성 검증
- 자동화된 초안 생성 방법론 연구

Chain-of-Draft는 효율적인 AI 추론을 위한 중요한 이정표를 제시하며, 실용적인 AI 시스템 구축에 있어 새로운 가능성을 열어줌 