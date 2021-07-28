# BlenderBot 1.0



## Introduction

* 지난 2년동안 Facebook AI Research(FAIR) 에서 발표한 작업을 융합한 결과물입니다.

* 개성(Personality), 공감(Empathy), 지식(Knowledge)과 같은 데이터셋에 Fine-tuning 한 모델은 챗봇이 더 사람다운 대화를 하도록 합니다.
* decoding 전략을 잘 사용하고 `retrieve-and-refine` 모델을 통해 지식에 대해 반복적이고 어눌하며 구체적이지 않은 답변을 피할 수 있습니다.



### Model

### 1-0 The Data(Good Conversation? Blended Skills!)

* 데이터셋에 대한 설명(ConvAI2, ED, Wizard Of Wikipedia, Blended Skill talk, Riddit)

#### 1-1. Retriever

##### Idea

* 모델은 bert-base(12개 Encoder block, 12개 Attention head, 768개의 hidden size) 를 사용했습니다.
* 입력으로 대화 context가 주어지면, candidate response 군에서 가장 높은 점수의 응답을 출력하는 Multi-Sentence Scoring을 Poly-encoder를 통해 수행하여 next dialogue utterance 를 선택합니다.
* Architecture 로 Poly encoder를 사용하고 데이터로 reddit, wikipedia, toronto book corpus 데이터를 사용하여 pre-train을 진행했습니다.
* Fine-tuning 은 ConvAI2 데이터로 진행했습니다. 
* Poly-Encoder는 256M, 622M 을 만들었고 하이퍼 파라미터인 Code는 64로 정했습니다.

  



#### 1-2 Generator

* 모델로는 standard seq2seq Transformer 와 BPE tokenizer를 사용하였습니다. 

* 여러 모델 사이즈를 고려하고 만들었습니다. (90M, 2,7B, 9.4B)

  * cf) Meena 2.7 B
  * 2.7 B 모델은 2개의 인코더와 24개의 디코더, 2560 임베딩 차수, 32개의 attention head로 대략 Meena 와 비슷합니다.
  * 9.4B 모델은 4개의 인코더, 32개의 디코더, 4096 임베딩 차수, 32개의 attention head로 이루어져 있습니다.



#### 1-3 Retrieve and Refine

* 앞서 언급했던 Retriever과 Generator를 결합한 구조입니다.

* 외부 데이터에 대해서 답변을 못하는 문제를 해결하기 위해 retrieve and refine 모델 이라는 생성 전에 retrieval 스텝을 결합하는 것입니다. => 이렇게 함으로써 생성 모델이 적절한 경우 검색 결과에서 단어나 구를 복사하는 법을 배울 수 있는것을 기대합니다.

* #### Dialogue retrieval

  * Input context + [SEP] + retrieved next utterance
  * retrieved next utterance를 그대로 출력하는 대신 Generator를 통해 응답을 생성하도록 합니다.
  * 이렇게 하는 이유는 Retriever만 사용하는것 보다 더 자연스러운 문장을 생성할 수 있고, Generator 만 사용 하는것보다 retrieved 된 next utterance는 사람이 작성한 gold response 이므로 더 명확한 단어, 표현은 Retriever를 통해 배울 수 있습니다.



* 명훈님 The Choice of Generation Strategy(Model Architecture, Training. Objective, Decodig)
  * Poly-Encoder, BART
  * Retrieve and Refine
  * Unlikelihood Training
  * Decoding Strategy
* Result * Analysis (Human Evaluation Using ACUTE-Eval)
  * ACUTE-Eval 평가 지표 소개 및 Ablation study, Meena 와 비교



### Training Objectives

#### 2-1. Ranking for Retrieval

* 검색모델을 학습하기 위해 Ycand1 은 정답 응답이고 나머지 샘플링된 negatives 인 샘플은 511개를 채워 한 배치당 512개로 학습을 진행했습니다.

#### 2-2. Likelihood Training for Generation

* standard maximum likelihood estimation(MLE) 방법을 사용합니다.

#### 2-3. α-blending for Retrieve and Refine(Dialogue retrieval)

* 





94억개 파라미터를 가지고 3가지(성격 + 지식 + 공감)를 조합한 Generator 문장 생성 ㅇ신경망 대화 모델

* 데이터 규모:









