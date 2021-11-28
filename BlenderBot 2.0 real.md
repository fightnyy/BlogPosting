# BlenderBot 2.0

* GPT3 와 Blenderbot 1.0 은 어제 대화 했지만 오늘 대화할 때 어제 대화한 내용을 잊어버리고 "hallucinated Knowledge" 즉, 정확하지 않은 지식을 의심없이 이야기 한다.

### 데이터셋

* 신경망을 훈련하기 위해 특히 크라우드소싱 플랫폼에서 훈련데이터를 수집했습니다. 또한, Wizard of the Internet과 Multi-Session Chat 으로 알려진 대화 데이터셋을 사용

1. 인터넷 검색의 새로운 정보로 사람 대화를 증가(Wizard of the Internet)
2. 대화 세션의 지식을 참조하는 사람과의 긴-맥락의 채팅인 멀티세션(Multi-Session Chat)

    * 첫번째 데이터셋은 Blenderbot 2.0 에 대한 관련 검색엔진 쿼리를 생성하는 방법과 검색 결과에 따른 관련 응답관리 기능을 제공합니다.
    * 두번째 데이터셋은 장기기억에 새로운 지식을 저장하는 챗봇에 대한 관리(Supervision)와 주어진 메모리와 관련된 응답을 제공하는 것에 대한 관리 기능을 제공합니다.

## 특징

* BlnederBot 2.0 은 인터넷 검색 쿼리를 생성할 수 있고 시간이 지나감에 따라 지식을 추가하면서 이전 아이디어를 참조할 수 있는 능력을 가진 최초의 챗봇이다.

    * Long-term Memory
        * BlenderBot 2.0 은 Long-term Momory 모듈을 구현한 덕분에 사용자와 며칠, 몇 주, 몇 달 동안 지속된 여러 대화들의 토픽들을 기억할 수 있다. 예를들어 사용자와 ㅏ몇 주
          전에 Tom Brady 에 대해서 이야기 했다면 미래 대화에서 NFL 언급을 할 수 있습니다.
    * Personalization
        * 대화 히스토리는 개인별로 분리되어 저장되며 한 사용자와 대화로부터 배운 새로운 정보를 다른 사용자의 대화에 사용되지 않도록 분리된다.
    * Internet Search
        * 대화 중에 BlenderBot 2.0 은 상황에 맞는 인터넷 검색 쿼리를 생성한 후, 그 검색 결과를 사용자의 질문에 대한 응답에 통합한다. 이렇게 하여 최신 정보를 유지할 수 있다.
        * BlenderBOt 2.0 은 새로운 지식을 검색하기 위해 Bing 검색 엔진을 통해 인터넷에 쿼리할 수 있으며 Long-term local memory 에 읽고 쓸 수 있습니다. 이를 통해 응답을
          조정할 수 있어 학습된 데이터 뿐 아니라 최신 스포츠 경기 결과ㅡ 속보 등을 반영하여 응답의 일관성을 유지하며 이야기 할 수 있습니다.

  참고 : BlenderBot 2.0 은 인터넷 정보를 적극 활용하기 때문에 영화감독 vs 프로듀서 , 투수 코치와 타격 코치 등과 같이 애매한 개념에 대해서 잘 혼동하지 않습니다.

## BlenderBot 2.0 의 모델 아키텍처

* 현 머신러닝의 추세는 상당한 컴퓨팅 자원이 필요한 Large 모델을 학습하는데 집중하고 있습니다. 이러한 모델들은 모델 Weight에 자신들이 학습한 것을 저장하려고 시도하지만 항상 변화하고 성장하는 인터넷의
  모든 정보를 저장하는것은 불가능에 가깝습니다. 따라서 Facebook 의 BlenderBot 2.0 은 인터넷의 정보를 모두 저장하는 것이 아닌 즉석해서 인터넷에 접근하는 방법을 사용합니다.

아래 그림은 BlenderBot 2.0의 구조 사진입니다.

![img](https://miro.medium.com/max/1400/0*A2FKdwuBj57TMFdt)

동장방식은 다음과 같습니다.

* 사용자와 대화 히스토리는 Blenderbot 2.0 의 Encoder와 Memory Decoder를 거쳐 Long-term memory에 저장됩니다.
* 사용자와 대화 중에 BlenderBot 2.0 은 Query generator를 통해 검색 쿼리를 생성한 후 인터넷 검색 및 Long-term 메모리로 부터 관련 정보를 찾습니다.
* 인터넷 검색 결과와 Long-term 메모리로 부터 찾은 결과는 Encoder로 입력 되고 Encoder의 출력들을 하나로 합친(concatenate) 후에 Decoder 로 입력된다. Decoder 는 최종
  응답을 출력한다.

### 시사점

* 기존 Chatbot 은 기억하지 않아 개인화가 어려웠던 문제가 있었다. BlenderBot 2.0 은 Long-term 메모리를 추가하여 Chatbot에 인간이 가지고 있는 기억을 부여하여 개인적인 대화가
  가능하여 인간과 닮은 대화가 가능해졌다.
* Long-term 메모리는 개인화를 위해 중요한 모듈로 Chatbot의 대화 히스토리가 축적하려면 모델 weight 외에 사용자의 히스토리를 저장 할 수 있는 추가적인 메모리 공간이 필요할 것으로 생각된다.
* 학습된 지식 외에 인터넷 검색을 통해 좀 더 정확한 응답을 선택할 수 있게 되었습니다. 이것은 모든 지식을 학습에 의존하지 않고 필요할 때마다 인터넷 검색을 통해 보충할 수있어 새로운 지식을 보충하기 위해
  Large 모델을 주기적으로 학습해야 하는 어려움이 크게 감소 할 수 있을 것으로 예상할 수 있다.

GPT-3 와 BlenderBot 1.0 은 진행중인 대화의 맥락에서 자신을 명확하게 표현하고 실제처럼 보이는 텍스트를 생성할 수 있습니다. 하지만 매우 짧은 기억력(goldfish memory - 금붕어 기억력)을
가진다는 점에서 한계가 있습니다.

또한 가지고 있는 장기기억이 고정되어 있기 때문에 대화 중 사용할 수 있는 소재가 이전에 배웠던 것들로 제한된다는 한계가 있습니다.

블렌더봇 2.0 은 페이스북의 RAG를 기반으로 한 모델을 사용합니다. RAG는 크게 Retriever과 Generator로 구성되며 특정 태스크에서 더 좋은 성능을 보였습니다. 즉 기본적인 seq2seq 모델
인풋에 문맥정보를 반영하여 더 정교하고 특정 도메인에 특화된 결과를 얻을 수 있습니다.

먼저 Retriever 은 큰 데이터셋(위키피디아)으로 사전학습된 문서 임베딩에 대한 인덱스를 가집니다. 여기서 인덱스는 아래 초록색 점의 DOC 'X' 를 의미합니다. 그 후 Question 인코더를 통해 '
Hemingway'가 입력되면 가까운 DOC 임베딩을 찾아 (ODC) 문맥정보를 만영합니다. 이 과정을 거치면서 원본 입력과 문맥정보가 연결되어 실제 출력을 생성하는 Generator 의 입력으로 입력됩니다.

![img](https://postfiles.pstatic.net/MjAyMTA3MjFfOTcg/MDAxNjI2ODQ1Nzk0NTg5.s4YGprsIR2rbZJrokyygu3FrgScC5lHfkPXUh4Sn3EIg.kq1NQ9m6AUhben1qOimWC0Km3hwpXJ76vBIKaFfMIlwg.PNG.dh0985/image.png?type=w773)

Generator 에선 원본 입력과 문맥 정보가 결합된 데이터를 입력으로 받아 marginalize 를 통해 하나의 타겟문장을 도출함. marginalize는 여러개의 확룰 변수로 구성된 조합 확률 분포에서 한 가지
변수에 대한 확률 값을 추정하기 위해 나머지 변수를 모두 적분해 제거 하는 과정을 의미합니다. 즉, 가장 높은 확률값을 결과로 도출하는 것이죠

![img](https://postfiles.pstatic.net/MjAyMTA3MjFfMjcz/MDAxNjI2ODQ1ODE3NjMz.9sh80QlG6rLyukO_6x4i0Ys-Q0-rw8-Vdhfo_pesJV4g.p3q4SylhvFaQmJHbDAHWwSo9tehxZfiwRGqftqmSRa0g.PNG.dh0985/image.png?type=w773)

이 접근법을 통하여 대화자체에 포함된 지식을 넘어서는 응답을 생성할 수 있습니다. 검색기와 seq2seq 생성기를 결합한 모델은 대화 중에 장기기억과 인터넷 검색을 통해 관련 정보를 찾을 수 있습니다.

* 이를 위해 대화맥락에서 관련 검색 쿼리를 생성하는 추가 신경망 모듈로 기존의 인코더-디코더 아키텍쳐를 강화했습니다.

* 그다음 FID 방법을 사용하여 인코딩된 지식을 대화내역에 추가합니다.

  ![img](https://postfiles.pstatic.net/MjAyMTA3MjFfMzMg/MDAxNjI2ODQ1ODQwNDM2.tKEiwxOWEG9gHmcSLrBhMJKLc6uRY23tPxrAERMvTvgg.MtQyHLLHIsEF61JiUOHJ_--uaj5tSmi9smX9kJ9MHCog.PNG.dh0985/image.png?type=w773)

### 실험결과와 Safety에 관한 이야기

실험 결과

- 블렌더봇 1.0 은 이미 Meena나 DialoGPT 와 같은 챗봇보다 성능이 매우 뛰어납니다. 따라서 새로운 모델을 평가할때 블렌더봇 1.0을 기준으로 하는게 가장 합리적으로 판단됩니다.
    - 블렌더봇 1.0과 비교 결과 블렌더봇 2.0의 경우 참여점수가 17% 나 향상되었습니다.
    - 이전 대화 세션 사용률은 __55%__ 로 블렌더봇 1.0 의 성능을 능가했고 또한 새로윤 시스템의 장기 기억 요소가 더 오랜시간동안 도 나은 대화를 지속가능하게 한다는것을 보여주었습니다.
    - 환각지식은 9.1 % -> 3.0% 로 사실에 일관된 대화는 12% 더 자주 하였습니다.

Safety

* 또한 Safety 문제도 현재 챗봇에 주된 키워드중 하나입니다. 페이스북 AI 팀은 대화주체의 안전에 대해 진지하게 고민하였습니다.
* 특히 자주는 아니지만 가끔 챗봇은 안전하지 않거나 모욕적인 발언을 생성하는것이 잘 알려진 사실이기 때문입니다.(의도적으로 불쾌한 응답을 이끌어 내려는 Prompt 를 입력할때 더 자주 일어납니다.)
* 이런 문제를 완화하기 위해 이용가능한 기술에 대해 대규모 연구를 수행했습니다. 그 결과 __내장 안정성(Baked-in safety)와 어려운 프롬프트(prompt)에 대한 견고함__ 을 개발하였습니다. 또한
  리서치 커뮤니티가 함께 발전할 수 있도록 돕기 위해 대화 안전 워크숍을 공동으로 준비하고 있습니다.
* 연구에 따르면 이런 접근 방식은 기존 기술보다 훨씬 우수했다고 합니다. 페이스북의 safety recipe를 실행함으로써 자동화된 분류기로 측정되는 불쾌한 응답은 90% 감소했고 실제 사람과의 대화에선 안정한
  응답이 74.5 % 증가했다고 합니다.
* 하지만 연구진은 아직 안전 이슈는 완전히 해결되지 않았다고 합니다. 또한 인터넷과 장기기억을 활용해 대화를 활용하는 블렌더봇 2.0의 접근법은 새로운 안전문제를 야기할 수 있습니다.

### 결론

- \- **블렌더봇2.0** 모델 혁신은 최신 SOTA 시스템보다 **중요한 진보**를 이뤘고 다른 연구자들 또한 **블렌더봇2.0**의 새로운 기능인 장기기억 구축과 인터넷 검색을 통해 지식을 추가하는 방법을
  사용할 수 있음. 물론, 해결해야할 문제들은 여전히 많음. 모델 환각은 줄었지만 일부는 남아 있으며 **모델들이 더 깊이 이해하기 전까지**는 그들 스스로 종종 모순될 수 있음

\- 마찬가지로 모델은 아직 **무엇이 안전한 것인지 완전히 이해하지 못함**. 그리고 장기기억을 구축하는 동안 엄밀히말해 학습하지 못하는데, 이는 모델이 실수를 개선하지 않는다는 것을 의미함. 하지만 이 분야에서
일부 진보를 만들어 냈음

\- **멀티모달
블렌더봇(**[Multimodal Blenderbot (parl.ai)](https://parl.ai/projects/multimodal_blenderbot/?fbclid=IwAR36rjWCcS5VtLuaxcfL6YOjhMuvMpaK0hgYcl1R__HHpxD1ZnYnP8YNbtc))으로
탐색해본 것처럼 페이스북 AI팀은 사람처럼 보고 말하며 의사소통하고 이해할 수 있는 에이전트가 금방 나오기를 기대함. 최신 기술을 혼합해 **단일 AI 시스템**으로 만드는 것이 블렌더봇 연구의 목표임. 챗봇의
향상은 가상도우미나 디지털 친구와 같은 응용분야의 SOTA를 더욱 발전시킬 수 있다고 생각함

![img](https://postfiles.pstatic.net/MjAyMTA3MjFfMTc3/MDAxNjI2ODQ5NDQzMTU2.P9bIIs2bDmbishrne4QPo6afYASw-Uv_TXyxszk37IYg.ADIUFcQZqtrtNnBjSXNq8cwhHEblKmOuzvW-KndXhIEg.PNG.dh0985/image.png?type=w773)
