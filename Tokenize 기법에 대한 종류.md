## Tokenizer에 종류

1. 단어 단위로 Tokenize
2. 글자단위로 Tokenize
3. 하위 단위로 Tokenize

#### 단어 단위로 Tokenize

* 장점 : 가장 간단하다(space 단위로 tokenize 를 하면 되니까)
* 단점
    * 비슷한 단어끼리 의미를 공유하는 단어의 의미가 없어지게 된다.
    * 단어가 굉장히 많기 때문에 거의 무한개의 토큰수가 생성된다.
    * 토큰수를 한정하면 vocab에 해당 단어가 없을 경우 OOV가 되기 때문에 실제로 굉장히 뜻이 다른 단어지만 같은 Representaion으로 표현된다.

#### 글자 단위로 Tokenize

![image-20210719161542967](/Users/nayeong-yun/Library/Application Support/typora-user-images/image-20210719161542967.png)

* 장점 :
    * 가장 적은 vocab 사이즈로 모든 표현이 가능하다.
* 단점 :
    * 각각의 토큰의 의미를 거의 가지지 않는다.
      ![image-20210719161838026](/Users/nayeong-yun/Library/Application Support/typora-user-images/image-20210719161838026.png)
    * 모델의 입력으로 들어가게 될 때 굉장히 긴 sequence로 들어가기 제약이 많다.

### subword 로 Tokenize(가장 좋음)

### 결론 : 자주 사용되는 단어들은 작은 subwords들로 나눠지는 것이 좋지 않다. , 자주 사용되지 않는 단어들은 의미있는 subwords들로 분리 될 수 있게하자.

![image-20210719162653459](/Users/nayeong-yun/Library/Application Support/typora-user-images/image-20210719162653459.png)

###  

### ETC

실제로 tokenize 된 결과를 보면

![image-20210719162838378](/Users/nayeong-yun/Library/Application Support/typora-user-images/image-20210719162838378.png)

이렇게 앞에 __##__ 가 붙은경우가 왕왕 보인다. 이는 단어의 시작이 아니라는 의미이다. 즉, __izaition__ 은 단어의 첫 시작이 ization으로 시작되었다는 의미이지만 __##ization__ 은
단어의 첫 시작이 다른 토큰으로 시작되었고 ization은 그 토큰 뒤에 나온다는 의미이다. 
