

### Towards a Human-like Open-Domain Chatbot

1. Human-like Sensibleness and Specificity Average
2. Open-Domain Chatbot - MEENA(Evolved-Transformer)



#### 첫번째 평가 척도(Sensibleness)

* 대화의 맥락에 따라 일관적이고 논리적이며, 사실에 기반한 대화를 나누는가?
* 평가요소 : common sense, logical coherence, consistency
* 챗봇 대화 예시
  * 너 운동 좋아해?
  * 응 운동 좋아해
  * 정말? 그러면 어떤 운동을 좋아해?
    * 나는 축구를 제일 좋아해(Sensibleness = 1) ; 일관적이기 때문에
    * 나 운동 안좋아해(Sensibleness = 0) ; 일관적이기 않기 때문에
* 한계: 의문문에는 "I don't know". 평서문에는 "ok"라는 대답(Sensibleness=1), human-like 지표로 불충분 



#### 두번째 평가 척도(Specificity) - 주관적

* 일반적이고 단조로운 대답이 아니라 진짜 사람처럼 대답을 하는가?
* 챗봇 대화 예시
  * 난 축구를 좋아해
    * 그렇구나(Sensibleness = 1, Specificity = 0)
    * 나도, 나는 리오넬 메시 완전 팬이야(Sensibleness = 1, Specificity = 1)



#### SSA(Sensibleness and Specificity Average)

따라서 MEENA에서 제안하는 평가 방식인 SSA는 챗봇의 human like 를 측정하는 방법이다

여기서 측정방식은 __Sensibleness와 Specificity Average 의 평균__ 이다.

__Sensibleness__ : concrete and basic human quality

__Specificity__ : more subjective human quality

여기서 Sensibleness 의 값이 1로 평가되면 Specificity 도 평가 되지만 Sensibleness 의 값이 처음부터 0 으로 평가되면 Specificity도 자동으로 0으로 평가됨 



#### Static Evaluation and Interactive Evaluation

*  정적평가(Static Evaluation)
  * 1~3 개의 턴으로 이루어진 1,477개의 대화 데이터셋(Mini-Turing Benchmark; MTB)
    * Single turn Context : 315, two-turn : 500, three-turn : 662
    * MTB 에는 Persionality Question 포함 (축구 좋아해?)
  * 성향에 관한 질문 personality question 에 대한 일관성 측정 가능
* 동적 평가(Interactive Evaluation)
  * 정적 평가는 데이터셋에 의해 편향이 생길 수 있음
  * 실험자와 챗봇이 1:1로 "Hi" 라는 대화와 함께 대화 시작
  * 도메인이나 주제에 대한 제한은 없음
  * 한 회당 최소 14에서 최대 28의 multi-turn 으로 구성
  * 각 모델 마다 100개의 대화 수집, 이를 바탕으로 Sensibleness 와 Specificity 평가



#### SSA consistency

* Crowd working
  * Crowd worker의 일관성을 측정하기 위해 agreement 와 Krippendorff's alpha 사용
  * 5명이 레이블링  하더라도 꽤 높은 일치율을 보임
* SSA 와 Human likeness 의 그래프를 각 축으로 비교해보니 일정한 상관관계가 나오는 것을 확인할 수 있었음 





### Types of Chatbot

* Close-domain
  * 특정 목표를 달성하기 위하여 keyword 또는 intent 에 대한 적절한 반응을 하며 대화가 진행되는 구조
    * 여행 관련 예약 시스템, 고객 서비스 시스템
* Open-domain
  * 불특정한 주제의 대화에 대해 사람의 대화와 비슷한 특성으로 대화가 진행되는 구조
    * Cleverbot, Mitsuku, 이루다, Meena