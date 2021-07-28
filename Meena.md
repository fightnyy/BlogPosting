# Meena

## 평가방식 

#### Static Evaluation

* 데이터셋 : Mini-Turing Benchmark (MTB)
  * 1477개의 1~3 턴의 멀티턴 대화 테스트셋을 미리 준비
  * 컨텍스트가 미리 정해져있기 때문에 "static evaluation"
* MTB 만든 방법
  * 315개의 싱글턴 문장을 Vinyals and Le (2015)와 뢰브너 프라이즈 콘테스트에서 추림
  * 해당 문장을 500개의 2턴과 (500 * 2), 662개의 3턴 컨텍스트로 만듦(662 * 3) -> 1477개
* 개인 성향이나 선호 관련 질문을 일관성을 확인하기 위해 포함시킴
  * A : 축구 좋아해? / B: 응. 난 토트넘 좋아해 / A: 정말? 제일 좋아하는 선수가 누구야?
    * B : 손흥민 좋아해 -> 일관성 있음 ->  sensibleness = 1
    * B : 영화 안 좋아해 -> 일관성 없음 -> sensibleness = 0



#### Interactive Evaluation

* Static Evaluation의 한계
  * Static evaluation은 모델을 비교할 때는 유용하지만, 미리 구축되어 있기 때문에 bias가 있을 수 있음.
  * 이 문제를 해결 하기 위해 Interactive evalution 을 만듦
* Interactive Evaluation의 정의 : 크라우드 소싱으로 마음대로 1:1 채팅을 하게 하는것
* 방법
  * 대화는 "Hi" 로 시작함
  * 그 다음 부터는 자유대화
  * 최소 14턴 (챗봇은 7턴) ~ 28턴의 대화를 하게 함
* 평가 
  * 각 모델에서 100개의 대화(즉, 적어도 700개의 챗봇 턴)를 대상으로 sensibleness 와 specificity를 레이블링

#### Human Performance Evaluation 

* 사람끼리 대화 시켜서 100개의 대화를 얻음
* 이를 대상으로 SSA 평가를 진행
  * 5명의 레이블러의 다수의 의견으로 평가

## TL;DR

* Google 에서 나온 End-to-end의 open-domain Generation 모델로 압도적으로 높은 수준 대화 구현
* 모델 
  * Evolved Transformer seq2seq model
  * 26억개의 파라미터
  * 1개의 ET encoder block + 13개의 ET decoder block
  * 히든 사이즈 2560, attention head 32
  * Encoder, Decoder 각기 128의 최대토큰을 가짐
* SSA(Sensibleness and Specificity Average)
  * Sensibleness 
    * 현재 컨텍스트를 놓고 봤을 때 말이 되는지 여부
  * Specificity
    * 대답이 구체적인지 평가하는 척도
    * GenericBot(평서문에는 "ok", 질문에는 "I don't know" 라고 대답하는 봇)과 DialoGPT의 sensibleness를 비교해보면 각각 70%, 62% 이지만 DialoGPT가 훨씬 말 잘함
  * Sensible 하다고 판단하면 그 이후로 Specificity 판별 
    * Sensible이 0이면, Specificity은 자동으로 0
    * Sensible이 좀 더 확실하고 기본적인 척도
    * Specificity는 좀 더 주관적인 척도
  * 사람 같은 대화임을 측정할 수 있는 메트릭임
* PPL이 챗봇의 성능을 나타내는데 가장 유의미한 지표(기계적으로 optimize 할 수 있기 때문에 실험과 모델 발전에 매우 유리)
  * 다양한 모델에 대해 SSA와 test perplexity를 측정해봤더니 대부분은 레이블링 variance가 perplexity로 설명이 된다는 걸 발견
    * Static SSA - Perplexity R-squared 0.94
    * Interactive SSA - Perplexity R-squared 0.93
* Decoding 방법 비교 
  * PPL이 낮다면 간단한 sample-and-rank decoding 전략으로도 다양하면서 높은 퀄리티의 답변을 내놓을 수 있다는 것을 보여줌 
  * <img width="2538" alt="image" src="https://user-images.githubusercontent.com/55227984/126917093-ed1d2494-9e39-44b1-92e8-5a9cc5b723c6.png">
  * 실험을 진행할 때 같은 모델(Evolved Transformer)을 사용했지만 sample-and-rank(meena가 사용한 방법) 과 beam search 를 사용한 결과가 완전히 다름
  * Beam search 가 Log-Likelihood 의 점수가 sample-and-rank 방식보다 훨씬 낮지만 만드는 문장이 재미가 없음
  * 하지만 sample-and-rank 방식을 쓸때 중요한 핵심은 __Perplexity(PPL)__ 이 충분히 낮은 모델을 만드는 것이다.
    * 그래야 해당 방법을 쓰더라도 여전히 말이 되는 문장이 나오기 때문

