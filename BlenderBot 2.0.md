# BlenderBot 2.0

안녕하세요. TUNiB의 NLP Engineer 나영윤이라고 합니다. 최근 FAIR(Facebook AI Research)에서 BlenderBot 1.0 의 문제들을 어느정도 해결한 BlenderBot 2.0을
발표하였습니다. 이번 블로그 글은 BlenderBot1.0 은 어떠한 문제점이 있었고 BlenderBot 2.0은 해당 문제를 어떻게 해결했는지 살펴보고자 합니다.

## 1. Long-term Memory 문제를 해결하지 못했다.

* Meena, BlenderBot 1.0과 같은 최신 Open-domain 챗봇은 어느정도의 공감능력과 사람 같은 대화를 생성해 내는 능력을 보여주었습니다. 또한 GPT-3는 충분히 많은 데이터와 모델 사이즈를
  키워 모델에 적절한 프롬프트를 주면 여러 태스크도 해결 할 수 있다는 가능성을 보여주었습니다.
* 하지만 이런 모델들도 Long-term Memory 문제는 해결하지는 못했습니다. Long-term Memory 문제는 챗봇과 사람이 이야기를 나누는 횟수가 길어질수록 이전의 내용을 챗봇이 기억하지 못하는 문제를
  말합니다. 첫번째 턴에 "내가 좋아하는 음식은 스테이크야" 라고 말하고 몇 턴이 지난후 "내가 좋아하는 음식이 뭐야?" 라는 질문을 하면 챗봇은 이를 기억하지 못하는 것이죠.

  ![image](https://user-images.githubusercontent.com/55227984/127774122-c7ee1e64-9f71-4e18-962b-f2d331e49815.png)
* BlenderBot 2.0 은 이 문제를 Retrieval 모델 구조로 해결하고자 했습니다. 모델로는 기존의 페이스북에서 발표했던 논문인 RAG, FiD 그리고 RAG 방식으로 훈련된 retriever 모델을
  가져온 RAG-FiD 가 있습니다.
* 하지만 어떠한 전처리를 하지 않고 대화의 history를 모두 넣을경우 모델의 입력 크기는 제한되어 있기 때문에 중요하고 핵심적인 정보가 모델의 입력으로 들어가지 못하는 문제가 생깁니다.
* 이 문제를 해결하기 위해 BlenderBot 2.0 은 encoder-decoder 모델을 두어 대화 history를 축약하여 저장하는 방식을 취했습니다.
  ![image](https://user-images.githubusercontent.com/55227984/127774514-f86c0967-c06a-491b-ab72-83cb4c3197a5.png)

* 위 표를 보면 크게 3부분으로 나뉜것을 알 수 있습니다. 첫번째 부분은 history를 retrieval 하지 않고 단순히 모델의 입력 길이를 늘리거나 Multi Session Chat 데이터셋으로
  fine-tune 한 모델. 두번째 부분은 retrieval 모델(RAG, FiD, FiD-RAG)을 사용한 모델들. 세번째 부분은 대화 history를 축약하여 저장하는 방식인 방법을 취한 모델들 입니다.

* 실험 결과 이전 대화의 history를 가져오는 방식이 그렇지 않는 방법에 Perplexity 점수가 더 낮은 것을 알 수 있습니다. 또한 대화 history를 단순 retrieve 한 모델 보다 history를
  축약하여 저장하는 모델이 Perplexity의 점수가 더 낮은 것을 알 수 있습니다.

    * 최종적으로 history를 축약한 Retrieval(SumMem-MSC 2.7B(FiD-RAG))를 사용한 결과 BlenderBot 2.0은 아래의 사진과 같이 대화 history를 충분히 기억하면서
      매끄럽게 대화를 이어나가는 결과를 보여줍니다.
    * ![image](https://user-images.githubusercontent.com/55227984/127774705-525b4faf-2e89-4280-96e3-77b76375b1b9.png)

## 2. 대화 내용을 최신상태로 업데이트 하지 못한다.

* GPT-3 와 같이 굉장히 많은 데이터로 학습한 모델은 모델 내에 여러 정보를 저장하고 있다는 사실은 널리 알려져 있습니다. 그렇지만 이러한 정보들은 최신의 정보는 반영하지 못한다는 단점이 있습니다.

  ​                            ![image](https://user-images.githubusercontent.com/55227984/127790358-b4b7a272-484a-4d02-9f32-d79ce5a31932.png)

* 위의 사진은 사용자가 BlenderBot 1.0에게 각각 최근 방영한 드라마 완다 비젼에 대한 자신의 생각을 말하고 있습니다.

* BlenderBot 1.0 이전에 완다 비젼을 학습한 적이 없기 때문에 해당 내용에 대해서 전혀 알지 못한다는 응답을 내놓는 것을 볼 수 있습니다. 하지만 BlenderBot 2.0은 이 문제를 해결하기 위해
  웹검색을 활용하여 대화를 생성합니다.
*
![image](https://user-images.githubusercontent.com/55227984/127790519-07f70e46-8382-4d86-a760-fdde234a12fe.png)

* Facebook 팀에서 맨 처음부터 웹 검색을 통해 문제를 해결하고자 하지는 않고 웹에 있는 데이터를 저장하여 대화 내용과 저장된 정보의 벡터유사도를 검색하여 정보를 찾는 방법(DPR + FAISS), 찾고자
  하는 정보에 대한 질문을 생성 후 정보를 찾는 방법(Search Query + FAISS )을 비교 해본 결과 웹 검색(Bing Search) 방법이 가장 효과적인 방법인 것을 알 수 있었습니다.

* 그 결과 아래의 사진처럼 BlenderBot 1.0 과 다르게 BlenderBot 2.0은 이전에 본 적없는 "Wandavision" 이 입력으로 들어와도 웹 검색을 통하여 더욱 더 적절한 대화를 생성해 내는
  것을 볼 수 있습니다.

  ![image](https://user-images.githubusercontent.com/55227984/127791851-faf5e9b9-7810-422b-8eec-c11e371e21a1.png)

## 마치며

BlenderBot 2.0 에 대한 내용을 보면서 Open-domain 챗봇에 여러 시도들이 제안되고 최종적으로 도달하고자 하는 "사람같은 챗봇"에 점점 가까워 지고 있다는 생각을 했습니다. 특히, 챗봇에 대한 __
Retrieval + Generation__ 모델과 __웹검색__의 여러 가능성을 확인할 수 있었습니다. 발표영상은 BlenderBot 2.0 뿐만 아니라 기존의 Open-domain 챗봇 모델이었던 Meena,
BlenderBot 1.0에 대한 설명과 더불어 Dens Passage Retrieval for Open-Domain Question Answering, Fusion in Decoder 와 같이 BlenderBot
2.0이 base로 사용한 모델도 설명하였습니다. BlenderBot 2.0 에 대한 더 자세한
내용은 [Facebook AI Research 블로그][https://ai.facebook.com/blog/blender-bot-2-an-open-source-chatbot-that-builds-long-term-memory-and-searches-the-internet/] 을
참고해주세요. 
