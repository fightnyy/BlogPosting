# Leverageing Passage Retrieval with Generative Models for open-domain Question Answering

#### keywords : generative model, Question answering, Passage retrieval

## TL;DR

* Big Generative Model 이 여러 QA 셋에서 좋은 성능을 보여줬었습니다.. 하지만 이렇게 LM 은 모델의 크기가 그만큼 많이 필요하기 때문에 cost가 상당합니다.. 이를 보안하기 위해서
  Retrieval 모델을 부착하여 적은 파라미터를 가진 LM 에서도 그만큼의 성능 냅니다.
* 해당 논문에서 모델은 크게 2가지 과정으로 이루어집니다..
    *
        1. Sparse 혹은 Dense Representation을 이용해서 support passages 를 retrieve 합니다.
        2. 그 후 Seq2Seq 모델은 Question 을 받고 supporting passage를 입력으로받아 처리 합니다.
* 해당 방법은 TriviaQA와 NQ벤치마크에서 Sota를 달성했습니다..
* 해당 방법의 성능은 retieve한 passage의 수에 따라 향상되며 이는 seq2seq 모델이 retrieved 한 passage를 바탕하여 더 좋은 답을 생성할 수 있다는 것을 의미합니다.

### Passage Retrieval

* passage 안의 정답인 span을 찾아내는 reader model이 모든 passage를 검색하게 하기는 많은 리소스가 들기 때문에 힘듭니다. 따라서 reader model 의 최적의 입력을 줄 수 있는 작은
  셋을 찾을 수 있는 효과적인 retriever component가 필요합니다. 여기서 Author 는 2가지 접근법을 선택합니다.

    * BM25 : Passage들은 bag of words 방식으로 표현되어 있고 이에 대한 ranking function이 TF-IDF 방식으로 구현되어 있습니다.
    * DPR :  Passage 와 Question이 dense vector로 representation 되어 있습니다. 해당 representation은 2개의 분리된 BERT 모델을 이용해서 얻어집니다.
      한개는 passage만을 encoding 하였고 다른 하나는 question만을 encoding 하였습니다. ranking function은 Question과 Passage 벡터의 내적으로 구현됩니다.
      Passage의 retrieval은 FAIR의 FAISS를 이용하여 구현됩니다. M개의 text pessage 가 있다고 가정할 때, DPR의 목적은 Run-time에 빠르게 입력 question과
      관련있는 top-k개의 passages 들을 낮은 차원의 연속적인 space로 모든 Passage 들을 index 하는 겁니다. (M은 몇백만개의 passage 들이 될 수 있고 k 는 그보다 상대적으로
      적은 20-100 개정도 될 수 있습니다.)

<img width="937" alt="image" src="https://user-images.githubusercontent.com/55227984/126923038-b897062c-7446-42b2-b9e3-987360f1475e.png">



DPR 의 핵심은 위의 그림에서 볼 수 있듯이

* 두개의 독립된 BERT 가 dense encoder로써 passage 뿐 아니라 question에 사용된다는 겁니다. Passage encoder는 어떤 test Passage에도 d차원 벡터로 맵핑됩니다. 이
  벡터가 retrieval에 사용될 index를 만들때 사용됩니다.
* DPR을 Train하는 것은 Question 과 passage의 내적이 검색에 활용될 수 있는 좋은 ranking function 된다는 것을 의미합니다. inference time에는 question 인코더가
  입력 question을 인코딩 하여 d 차원 벡터로 만들고 해당 벡터를 활용하여 question vector와 가장 가까운 k개의 passages 를 찾는데 사용됩니다. DPR에서 사용되는 similarity
  function 은 'dot-product'(내적) 이지만 'decomposable similarity' 함수와 같은 변형된 유클리드 거리도 사용될 수 있습니다.
* Indexing 과 Retrieval 할 때, FAISS 라이브러리가 사용됩니다.

### Generative model for Answer Generation

* 해당 논문에서 사용한 generative 모델은 T5를 사용하고 있습니다. T5 의 구조는 Transformer와 일치합니다.
  ![image](https://user-images.githubusercontent.com/55227984/126925049-16d9f0c5-7425-4797-bd60-f7e4847949c7.png)
* Question, title과 함께 DPR을 사용해 검색된 각 passage는 Transformer 인코더에 들어갑니다.

> Encoder
>
> input : Question + title of the support passages + retrieved support passages
>
> * Question 은 "question" 이라고, Titile 은 "titile" 이라고 Passage는 "context" 라는 prefix가 붙습니다. 각 passage와 title은 question과 concatenated 되어 Encoder로 들어 갑니다.
> * 연관된 Passage 들을 독립적으로 처리하는 것은 더 많은 수의 context로 확장할 수 있게 하고 context 수가 증가하여도 computation time이 linear 하게 증가합니다.

> Decoder
>
> * 디코더는 모든 Retrived passage concatnation인 Result representation 에 어텐션을 수행합니다. 이 방식은 모델이 디코더에서만 evidence(Passage) 를 fusion하기 때문에 Fusion-in-Decoder 라고 불립니다.

<img width="896" alt="image" src="https://user-images.githubusercontent.com/55227984/126927954-2b0d5385-b086-4d2f-b66d-330192a8ac1d.png">

위 그림에서와 같이 __Question__ 에 대해선 DPR이 연관된 K개의 __Passage__ 를 찾아내고 Transformer Encoder의 입력으로 위에 서술한 형식으로 들어갑니다. Encoder의 출력으로
나온 representation은 concat 되고 해당 벡터는 디코딩을 할 때 attend 됩니다.

## 결과

<img width="852" alt="image" src="https://user-images.githubusercontent.com/55227984/126928450-00145e7a-b780-4102-95aa-0d1ccc24ea14.png">

* NQ(Natural Questions) : Google 검색에 사용되었던 질문들입니다. Open-domain version에서는 정답이 5토큰을 넘으면 데이터셋에 넣지 않았습니다.
* TriviaQA : Trivia와 quiz-league 웹사이트에서 모은 question
* SQuAD v 1.1 : 독해 데이터셋입니다. 위키피디아에서 얻은 단락이 주어지면 질문을 annotator들이 작성한 데이터셋입니다.
* EM : 정규화후 생성된 answer가 정답 목록의 어떤 answer와 일치하면 맞다고 표시
    * Ex. (generated answer : __Apple__, Answer : __Apple__ is the fruit that ......)
* SQUAD 를 제외하고 retrieve 할 때 DPR을 사용했습니다..
* Decoding 방식은 greedy 방식을 사용했고 모델은 Huggingface T5 checkpoint를 사용했다. 
