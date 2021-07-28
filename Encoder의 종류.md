# Encoder의 종류

##### 참조

![image](https://user-images.githubusercontent.com/55227984/126966071-2eda5a41-fc97-45bb-a0a3-0b3a3537ab7a.png)



### Bi-encoder 

* context와 candidate를 인코딩 하는 인코더가 분리되어 있습니다.
  * 장점: candidate representation은 캐싱되기 때문에 inference가 빠릅니다

  * 단점 : 정확도는 Cross-encoder에 비해 낮습니다.

  * 형태 :

    ![image](https://user-images.githubusercontent.com/55227984/126965400-9e687e6b-2eaa-45d0-b535-784607db70d2.png)

  * 위의 그림처럼 Context Encoder와 Candidate Encoder 가 각각 context 문장과 해당 context 다음에 올 후보 문장을 인코딩하는 구조입니다. 수학적으로는 다음과 같습니다.

    ![image](https://user-images.githubusercontent.com/55227984/126967372-a131a55d-fdf3-4ec0-8e82-ade2b3ce022f.png)

    * T(x) = h1,...,hn는 Transformer Encoder의 output을 의미하며, red( )는 이런 sequence output을 하나의 벡터로 변환시킵니다.
    * 최종적으로 얻어지는 y는 각 Encoder의 Contextualized Embedding을 의미합니다 .

  * 인코딩 결과로 Context Embedding, Candidate Embdding 을 얻을 수 있고, 두 백터의 내적을 통해 다음 문장으로 적절한가에 대한 점수를 수학점으로 다음과 같이 계산합니다. 

    ![image](https://user-images.githubusercontent.com/55227984/126984606-0521a4ed-d436-41e8-a570-20903628e9e5.png)

  * 위와 같은 방식으로 주어진 n 개의 후보들에 대해 점수를 구하고, 가장 높은 점수의 후보 문장을 주어진 context 다음에 올 문장이라고 간주합니다. 

  * 학습시에는 batch 내의 샘플을 negative로 사용해 cross entropy를 최소화시키도록 구성합니다.

    * 하나의 배치엔 [(ctxt1, cand1), (ctxt2m cand2), ... , (ctxtn, candn)] 와 같이 구성되고 이때 첫번째 ctxt1 에 대한 loss 는 cand1은 positive 나머지는 negative로 하여(target 을 [1, 0 ,0 ,.....0] 하여 계산한다는 의미입니다.)  cross entropy loss 로 계산 할 수 있습니다.

  *  Bi-encoder는 context 와 candidate를 독립적으로 인코딩 하기에 실제 서비스를 위해 inference 할 때 각 문장들의 embedding을 미리 계산해 둘 수 있다는 장점이 있습니다.



### Cross-encoder

* 최종 representation을 얻기위해 context와 candidate를 결합해 인코딩합니다(기존 BERT의 형태).

  * 장점: self-attention 과정에서 두 sequence가 서로를 token-level로 참조 할 수 있기 때문에 Bi-encoder에 비해 정확도가 높습니다.
  * 단점 : Bi-encoder 처럼 candidate representation을 캐싱하지 않고 pair에 대한 인코딩을 해야하기 때문에 inference가 매우 느려 서비스 이용엔 적합하지 않을 수 있습니다.

  ![image](https://user-images.githubusercontent.com/55227984/126965765-a692b9ea-ae5d-4518-a269-cf731e816d29.png)

* 학습은 위의 Bi-encoder와 동일하게 batch 내의 샘플을 이용하여 c ross entropy loss 를 최소화 시키는 방법입니다.

### Poly-encoder

* Bi-encoder와 Cross-encoder의 장점만 합한 형태

  * Inference 단에서 Cross-Encoder 보다는 빨라지고 Bi-Encoder 보다는 더 좋은 accuracy를 갖는다.

  ![image-20210726210544319](/Users/nayeong-yun/Library/Application Support/typora-user-images/image-20210726210544319.png)
  
* Bi-encoder 처럼 context와 candidate 를 독립적으로 encoding 합니다. 하지만 Context encoder 위에 있는 구조가 조금 다릅니다. 

  * 기존 context encoder의 output은 red()를 통해 바로 하나의 벡터로 합쳤지만, Poly encoder 는 code vector 와의 attention을 통해서 여러개의 벡터를 만들어 냅니다. (참고로 Candidate Encoder는 동일합니다.)

    ![image-20210726211451997](/Users/nayeong-yun/Library/Application Support/typora-user-images/image-20210726211451997.png)

  * 이때 code vector는 일종으 ㅣlatent variable 이라고 볼 수 있고 학습초기에는 Random initialize 되고 학습 과정 중에 learnable parameter 로써 함께 학습됩니다.

    *  Code vector 는 의미적으로 해석해보면, context의 여러 의미를 포착하는 역할을 한다고 볼 수도 있습니다.

  * 이렇게 얻어진 벡터들에 대해서(Emb 1, ... Emb m) Candidate Embedding 과의 attention을 통해서 한 번 더 벡터들을 합치고 최종  Context Embedding 을 구합니다. 최종 식으로는 아래와 같겠습니다.

    ![image-20210726211409935](/Users/nayeong-yun/Library/Application Support/typora-user-images/image-20210726211409935.png)

  * 최종 score는 위에서 구한 Context Emdding Candidate Embedding 의 내적을 통해서 계산합니다. 

  * 이렇게 하면,Cross-encoder 처럼 context 와 candidate 간의 관계를 보다 깊게 파악 할 수 있으면서, Bi-encoder 처럼 문장들의 Enbedding을 일정부분 미리 계산 할 수 있어서 inference 상황에서 매우 빠르고 효과적입니다.



### Inference

* 실제 서비스에선 수백, 수천만개의 후보 문장들 중 다음에 오기 적절한 문장을 inference 를 통해 찾아야 합니다.
* 이때 Bi-encoder 방식은 inference speed 가 매우 빠르다는 장점이 있습니다.
  * 후보 문장이 아무리 많아도 미리 인코딩을 해서 문장 별 Candidate Embedding을 계산 해 둘 수 있기 때문입니다.
  * 그러면 Query로 들어오는 Context 만 딱 한번 인코딩해서 Context Embedding을 계산하고 미리 계산해둔 수백, 수천만개의 Cnadidate Embedding 과 내적 계산만 하면 됩니다.
    *  Transformer Encoder를 inference 하는 비용해 비하면 두 벡터의 내적계산은 매우 빠르기 때문에 인코딩을 최대한 안하는것이 속도 향상의 포인트 입니다.
* 반면 cross-encoder는 상상할 수 없을 정도로 느립니다.
  * 수 백만, 수 천만 Candidate를 context 마다 concatenation 하여 쌍으로 인코딩을 해야하기 때문입니다.
* Poly-encoder 는 둘의 장점을 모두 취합니다.
  * Context와 candidate는 독립적으로 Transformer Encoder를 타기 때문에 Candidate Embedding을 미리 계산하여 구해 둘 수 있습니다.
  * 실제로 query 로 들어온 context에 대해서 한 번 inference를 하고, 미리 계산해 둔 Candidate Embedding 들에 대해서 attention 만 몇번 해서 최종 Score를 계산 할 수 있습니다.
    * 위 설명처럼  attention을 몇번 더 해야 하기 때문에 속도는 약간 느리지만 성능은 대폭 향상 됩니다.
    * Bi-encoder와 달리 최종 score 계산 전에 context와 candidate를 아우르는 attention 연산을 하기 때문에 둘의 관계를 좀 더 잘 이해할 수 있게 되고, 당연히 보다 적적한 다음 문장을 찾을 수 있습니다. 



### Expriment & Result 

![image](https://user-images.githubusercontent.com/55227984/126989326-64799be7-9e5c-40ce-ab58-4ae084057ffa.png)

* 성능은 Cross-encoder 가 가장 좋지만 Poly Encoder 도 Bi-encoder에 비해 성능이 대폭 향상된것을 알 수 있습니다.

![table2](https://roomylee.github.io/assets/images/blog/2020-08-11-poly-encoder/table2.png)

* 흥미로운 점은 batch size를 크게 할 수록 성능이 올라갔다는 것입니다. 즉 많은 오답중에 정답 한개 인것을 찾는것이 더 학습에 긍정적인 영향을 준것임을 알 수 있습니다.

![image](https://user-images.githubusercontent.com/55227984/126989640-2d60d7b6-7104-4077-9b16-43c765896cd0.png)

* 서비스에서 Cross-encoder 는 사용하면 안되겠습니다......
