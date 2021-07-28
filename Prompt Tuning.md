# Prompt Tuning



```
논문에 들어가기 전에 용어정리

* model tuning : fine-tuning
```



### Abstraction

> * discrete prompt 는 한계가 많은 접근 방법이다.
>   * 최적의 prompt가 뭔지 사람이 알기는 불가능하다.
>   * prompt가 너무 길 경우 입력이 불가능하다. (토큰의 제한갯수 때문에)
> * 따라서 해당 논문에선 soft prompt tuning이란 것을 소개한다
>   * GPT-3의 `few shot` example을 큰 점수차로 이겼다.
>   * 큰 모델(GPT-3) 같은 경우 transfer learning이 매우 힘들 뿐만 아니라 각 down-stream task 마다 각각 fine-tune 할 경우 많은 리소스가 필요하지만 prompt tuning은 모델이 pre training 한 파라미터는 다 freeze 시키기 때문에 학습시키는데 많은 리소스가 필요하지 않다.
> * `prefix-tuning`의 간단 버전이라고 생각하면 된다.



### Introduction

>* ELMo(Peters et al., 2018) 는 pre-trained 된 모델을 freeze 하는 방법을 제시 했지만 BERT(Devlin et al., 2019) 에서는 각 Task 마다 모델의 모든 파라미터를 업데이트 하는 fine-tuning 방법을 사용했다.
>* 이후 GPT-3와 같은 모델의 파라미터가 상당히 커지자 Brown et al.(2020) __prompt design__ (priming) 이라는 방식을 선보였다. 
> * prompt 는 설명 또는 예시가 담긴 텍스트를 의미한다.
> * GPT-3 가 나온 이후로 하나의 Generalist 모델이 다른 여러 task를 해결할 수 있다는 것이 어필되었다.
>* 하지만 __prompt design__ 방식은 여러 한개가 존재한다.
> * 사람의 resource가 필요하고 사람이 넣는 prompt가 오류를 범하기 쉽다.
> * 입력으로 받을 수 있는 토큰의 갯수가 모델에 정해져 있기 때문에 이를 넘어가는 prompt를 적을 수 없다.
> * Fine-tune 된 모델의 성능을 한참 따라올 수가 없다.
> * GPT-3 는 T5-XXL 에 비해 16배 많은 파라미터가 있지만 SuperGLUE에서 17.5점이나 성능이 뒤쳐저 있다.
>* 따라서 prompt를 자동화하기 위하여 여러 노력이 있었고 일정부분 성능도 올랐지만 아직 여러 한개가 존재했다. 
>* 이후 `prefix tuning`이라는 기법이 등장하였고 generative task에서 의미있는 결과를 보였다.
> * `prefix tuning` 이란 LM의 parameter들을 freeze 시키고 인코더와 입력 layer의 앞에 prefix activation을 붙여 여기서 발생된 loss를 활용하여 학습하는 방법이다.
> * 이 방법을 단순화하여 Hambardzumyan et al.(2021) 는 classification task 에서 좋은 성능을 기록했다.
>* __Prompt tuning__은 다른 모델 파라미터들을 다 freeze 하고 각 downstream task 마다 학습 가능한 k개의 token을 추가해주는 방식이다.
>  * 이 방법을 사용하여 __모델이 커지면 커질 수록 fine tune __ 비슷한 성능을 기록했다. 
>* 해당 논문의 key contribution은 다음과 같다
>  1. 모델이 커지면 커질 수록 prompt tuning이 fine tuning과 비슷해 진다. 
>  2. prompt ensemble 이라는 기법을 제안하고 이것에 대한 효용성을 보인다.
>  3. Big-LM(GPT3-3) 와 같은 모델의 현실적인 학습방법인 prompt tuning 이란 방식을 제안한다.





### Prompt Tuning

* 
