---
layout: post
title: Deep Learning을 위한 NLP 개요 정리.
subtitle: Image Processing과 비교
tags: [Machine_Learning, NLP]
image: /img/NLP-image.jpg
comments: true
---

대학원에서 픽셀 단위의 영상을 주로 다루다가 회사에 와서 자의 반 타의 반으로 공부하게 됐다. 영상을 다루면서는 '텍스트야 뭐.. 별거 있겠어?' 했었는데 직접 겪어 보니 NLP는 생각보다 참 복잡하고 어려운 분야다. 또한 영상만큼이나, 아니 그보다 더 역사가 깊은 분야이기에 본격적으로 뛰어들기 앞서 공부해야할 기반지식들이 많다. 

## Tokenize

영상을 이루는 픽셀값은 한정적이며 세계 어디에서나 공통된 의미를 지니고 있다. 그래서 별다른 처리를 하지 않고 RGB 3차원의 데이터 그대로 사용할 수 있었다. 하지만 텍스트의 ASCII 혹은 UNICODE 값의 의미 정보는 이와 다르다.

예를 들어, `(255,0,0)`의 RGB값은, 불꽃, 피의 색깔과 같은 공통적인 의미 요소를 유니버셜하게 가지지만 `97`의 ASCII 코드 값은 함유한 의미 요소가 적을 뿐만 아니라, 언어에 따라 사용 유무도 차이가 있다. NLP에서 최소한 픽셀 정도의 의미를 가지는 단위를 얻기 위해서는 전처리가 하나 더 필요하다. `Tokenize`다. 

최소한의 의미 정보를 포함하는 데이터를 얻기 위해, 보통 문단 단위로 이루어진 텍스트 데이터 말뭉치를 적절한 단위로 적절히 잘라낸다. 여기서 적절한 단위는 보통 어근이나 형태소가 된다. '적절한 단위'를 어떻게 볼것이냐가 Tokenize 기법의 핵심이다. 적절함을 너무 낮게 설정하면 의미 없는 정보들도 다 남게 되어 이후의 모델링에 혼란을 줄 것이고, 반대로 너무 높으면 문장의 일부 정보가 훼손될 수 있다.

[김현중님의 블로그](https://lovit.github.io/nlp/2018/04/02/wpm/)에 최근 자주 쓰이는 Tokenize 방법인 wordpiece 모델에 대한 좋은 설명이 나와있으니 참조하자.

문제는 언어마다 효과적인 Tokenize 방법이 조금씩 다르다는 것이다. 영어에서 아주 좋은 성능을 발휘하는 방법인 BPE(Byte Pair Encoding)라도, 한글에서도 동일한 성능을 발휘할 것이라고 100% 장담할 수는 없다. 언어학적인 지식이 필요한 대목이다. [Wiki](https://namu.wiki/w/%EB%B6%84%EB%A5%98:%EC%96%B8%EC%96%B4%ED%95%99)에 따르면, 영어는 고립어, 한국어는 교착어, 독일어는 굴절어로 분류되어 각각의 언어마다 독특한 특징이 있기 때문에 모든 언어를 아우르는 모델을 만들기 어렵다. (그래도 구글은 해낸다.. [bert-multilingual](https://github.com/google-research/bert/blob/master/multilingual.md))

## Embedding

Tokenize로 텍스트 구성 성분 중, 최소 의미 단위를 추출해내면 이것을 본격적으로 컴퓨터에 입력하기 위해 벡터화하는 작업이 필요하다. Tokenize가 끝나고 아직 Text였던 데이터를 N차원의 벡터로 변환하는 작업이 바로 `Embedding`이다.

애초에 RGB값이 숫자로 표현되어져 있는 영상처리에서는 고려하지 않아도 될 부분일 수도 있다. 굳이 따지자면 어떤 주파수 도메인에서 볼 것인지를 선택하는 것 정도가 되겠다.

Embedding은 크게는 One-hot encoding 방식처럼 단순히 text를 컴퓨터가 다루기 좋은 일정 형식의 숫자로 바꿔주는 행위를 모두 포함한다. 보통 [Word2Vec](https://ratsgo.github.io/from%20frequency%20to%20semantics/2017/03/30/word2vec/), [Glove](https://lovit.github.io/nlp/representation/2018/09/05/glove/)와 같이 유사한 단어끼리 비슷한 벡터값을 가지도록 하는 경우가 많고, [ELMO](https://wikidocs.net/33930)등에서는 단순히 단어가 아닌, 문맥으로부터 벡터값을 계산한다. 

Keras나 Pytorch에서는 text data를 정수에 매핑하여 간단하게 `Embedding()` 함수로 처리해버릴 수도 있지만, 최근에 등장하는 State-of-art급 논문들에는 단독 논문거리는 아니더라도 자잘하게 Word Embedding 부분을 커스터마이즈하여 성능을 높이는 시도가 계속되고 있으니 주목할 부분이다.

## Model

NLP 모델 개요는 최근 정리한 필기 스크린샷으로 대체한다.

<img width="739" alt="NLP 딥러닝 모델 개요" src="https://user-images.githubusercontent.com/12293076/62027289-40585200-b218-11e9-8583-70d50fdd12b4.png">

2012년 Neural Network 기반의 NLP로 넘어오기 전까지는 [Naive Bayes](https://gomguard.tistory.com/69)등의 주로 출현 빈도에 의존하는 통계 중심 모델들이  좋은 성능을 발휘했다. 하지만 LSTM등의 Recurrent 모델들이 통계 중심 모델들을 한참 따돌리는 성능을 내면서 NLP 분야의 패러다임이 바꼈다.

 2017년 ['All you need is Attention'](https://arxiv.org/abs/1706.03762)이라는 이름의 논문으로 공개된 Transformer를 기점으로, 현재는 Attention기반의 모델들이 Human performance를 능가하는 성능을 발휘하고 있다. 변화가 워낙 빨라서, 따라잡으려면 항상 신경을 곤두세우고 있어야 할 듯 하다.

 NLP에서는 영상처리쪽과 다르게 분류, 생성 모델에 대한 구분이 명확하지 않은 것 같다. 그도 그럴것이, Q&A나 번역등의 일부 NLP Task들은 자연스럽게 생성 부분이 들어가 있다. 잘나가는 모델들의 구조가 대부분 Auto-Encoder인 것도 흥미롭다. NLP에서는 feature를 뽑아내는 과정이 복잡하고, 추출한 정보를 재구성하는 과정에서 단어 선택이나 배치등의 조건이 영상보다 더 까다롭기 때문에 GAN보다 Reliable한 AE가 좋은 성과를 얻고 있는게 아닌가 하는 생각이 든다. 한번 생각해볼 주제일 것 같다.

## 좋은 Blogs and Web Pages

* [딥러닝을 이용한 자연어처리 입문 Wikidocs](https://wikidocs.net/book/2155)
* [딥러닝을 활용한 자연어처리 GitBook](https://kh-kim.gitbook.io/natural-language-processing-with-pytorch/)
* [Transformer](https://nlpinkorean.github.io/illustrated-transformer/)
* [BERT](http://docs.likejazz.com/bert/#input-embeddings)
* [XLNET](https://www.notion.so/XLNet-Generalized-Autoregressive-Pretraining-for-Language-Understanding-19-06-25-f4b608f11dfc4c8c8eb4c504f867d4aa)

## 마치며.

NLP를 주로 처리하는 회사에 오면서 급하게 공부했었던 내용을 필기장에 썩혀두다가 최근 여유가 조금 생기면서 블로그로 옮길 수 있게 되어 기쁘다. 사실 여유는 만들면 생기는 것이지만 매번 귀찮음에 굴복하곤, 여유 없음에 핑게를 찾았던 것 같다. 이미 서울에 자리를 잡은 이상, 귀찮음과의 싸움은 계속해야할 듯 하다. 높은 승률을 유지하는 것만이 살길이다!


*만약 내용 중, 오류가 있다면 댓글을 통해 알려주시면 감사하겠습니다.*
