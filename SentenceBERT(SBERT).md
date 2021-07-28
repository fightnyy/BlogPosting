# SentenceBERT(SBERT)



## Introduction

* BERT 네트워크에 siamese, triplet 구조를 적용했습니다. 이로인해 기존 BERT 가 하지 못했던 large-scale의 similarity comparison, clustering, information retreval 등을 할 수 있습니다. 
* BERT로 유사한 두 문장을 찾으려면 두개의 문장을 한개의 BERT 모델에 넣어야 유사도가 평가된다. 
* 따라서 문장이 10000개 있으면 (10000 * 9999) / 2 번의 연산 후에야 랭킹을 할 수 있습니다. 
  * ex. (1번 문장, ....., 10000번 문장 이렇게 있다고 가정해보면 1번문장은 2번문장, 3번문장 ....., 10000번문장과 모두 유사한지 찾는 비교를 해봐야하고 2번 문장도 쭉......)
*  Clustering 이나 search에서는 각 문장을 벡터 공간에 매핑하는 작업을 보통 쓰며, BERT를 이용할 때는 출력을 평균 내거나 [CLS] 토큰의 출력값을 이용한다. 
*  하지만 위 방법의 경우 각 단어의 GloVe 벡터를 평균 낸것보다 좋지 않다.
*  이에 siamese(샴) 네트워크로 __문장을 벡터화할 수 있는 SBERT__ 를 제안한다.
*  SBERT를 SNLI 데이터로 fine-tune 했습니다. 



### Model

![image](https://user-images.githubusercontent.com/55227984/126981659-6ba82ec5-b985-4ff7-ad6f-f89abd6a9fde.png)

* BERT에서 fixed-size sentence embedding 을 얻기 위해 풀링을 사용한다.(차원을 줄위기 위해서)

* 방법은 총 3가지 이며 [CLS]토큰 값 이용, 평균, max-over-time 방법이 있습니다. 

* SNLI 데이터로 fine-tune 할 때의 구조는 위와 같습니다.

* 두 문장의 출력값 인 u,v 그리고 element-wise 차이값인 |u-v| 를 concatenate 한 후 파라미터를 추가하여 학습힙니다. 형태는 아래와 같습니다.

  ![image](https://user-images.githubusercontent.com/55227984/126982134-fbcc2a08-8566-4040-8820-e840ab223ebb.png)
  
* 실제 inference 할 때나, regression 방식의 loss function을 쓸 때는 cosine-smilarity를 이용한다. Training 할 때, 계산된 cosine similarity와 gold label 간의 MSE 를 minimize 하는 방식으로 학습했다.

  ![image](https://user-images.githubusercontent.com/55227984/126982374-bc0912b7-c648-4e1b-be36-7340d7ecaf95.png)





### Conclusion 

* BERT로 STS를 했을 경우 성능이 좋지 않았다. 
* 그래서 샴 구조를 이용하여 fine-tuning 할 수 있는 SBERT를 제안했고, 성능 향상이 있었다.
* Computationally efficient 하며, BERT로 하기 힘들었던 clustering, retrieval 등을 할 수 있게 되었다.  
