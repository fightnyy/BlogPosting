# Just Say No : Analyzing the Stance of Neural Dialogue Generation in Offensive Contexts



### Abstract

* 공격적인 대화 모델의 응답을 연구했다. 공격적인 레딧 대화 데이터를 가지고 
* 새로운 데이터셋도 만들었다.(2,000쓰레드) - TOXICHAT 이라고 명명함
* Pre-trained 된 트랜스포머 classifier로 공격적ㅇ니 labels 에 대해서 0.71로 측정됨
* 또한 기존에 있던 controlable text generation 방법을 이용해서 공격적인 응답을 완화하기 위해 사용함
* baseline 과 비교해서 우리의 모델이 29%더 적은 공격응답 발화 사용함



### Introduction

* GPT3 에게 적절한 프롬프트를 줬을 때 Dialogue agent로서 매우 놀라운 능력을 보여주지만 모델이 어떻게 발화 할지를 모름
* text가 하나 있을때는 괜찮은 문장도 문맥에 따라서 공격적인 발화로 보이기도 함 (Dialogue agent 는 Hate Speech에 종종 동의하는 답변을 함)
  * Thank you for saying what I was thinking
* 현재 GPT3나 Facebook blender 챗봇같은 경우 공격적인 입력이 감지 되면 응답을 생성하지 않는 방법을 취합니다.
  * 근데 매우 부적절한 방법, Classifier는 완벽하지 않기 때문에 
  * 그거보단 공격적인 발화에 대해서 동의를 하지 않는게 더 선호됨