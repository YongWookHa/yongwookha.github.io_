---
layout: post
title: Interview Question & Answer
subtitle: 출근 루틴, 하루 3문제
tags: [MACHINE_LEARNING, IT, DEVELOP]
image: https://assets.rappler.com/C39701B689414EE2858D1ED6A10DF1A0/img/D067EDAB25364E8CA6AFBB7537387778/1-job-interview-tips-impress-20150308.jpg
comments: true
---

항상 양질의 글을 읽을 수 있어 즐겨찾는 [zzsza(변성윤)](https://github.com/zzsza)님의 블로그에서 [Datascience-Interview-Questions](https://zzsza.github.io/data/2018/02/17/datascience-interivew-questions/) 포스트를 발견했습니다. 공유되어 있는 양질의 문제들을 보며 출근 루틴으로 2~3문제씩 답안을 만들어야겠다는 생각이 들었습니다. 원문에는 다양한 도메인에 대한 질문들이 있는데 그 중, 관심을 가지고 있는 몇 가지 주제에 대해서 공부하고 나름대로 답안을 작성하여 기록하고자 합니다.

_항상 좋은 글로 영감을 주시는 변성윤님 감사합니다._

## Intro

Question by [Seongyun Byeon](https://github.com/zzsza)  
Answer by [YongWook Ha](https://github.com/YongWookHa)

답안은 `펼치기/접기` 형식으로 문제 아래 작성하고 있습니다. 각 문제의 컨텐츠를 매일 꾸준히 공부하고 답안을 채워나갈 예정이라 컨텐츠 완성까지는 꽤 시간이 소요될 것 같습니다.   
답안에 오류가 있거나 부족한 점이 발견된다면, 댓글 남겨주시기 바랍니다.

## Contents

- [통계 및 수학](#통계-및-수학)
- [분석 일반](#분석-일반)
- [머신러닝](#머신러닝)
- [딥러닝](#딥러닝)	 
	- [딥러닝 일반](#딥러닝-일반)
	- [컴퓨터 비전](#컴퓨터-비전) 
	- [자연어 처리](#자연어-처리)
	- [강화학습](#강화학습)
	- [GAN](#GAN) 
- [추천 시스템](#추천-시스템)


## 통계 및 수학
- 고유값(eigen value)와 고유벡터(eigen vector)에 대해 설명해주세요. 그리고 왜 중요할까요?
- 샘플링(Sampling)과 리샘플링(Resampling)에 대해 설명해주세요. 리샘플링은 무슨 장점이 있을까요?
- 확률 모형과 확률 변수는 무엇일까요?
- 누적 분포 함수와 확률 밀도 함수는 무엇일까요? 수식과 함께 표현해주세요
- 베르누이 분포 / 이항 분포 / 카테고리 분포 / 다항 분포 / 가우시안 정규 분포 / t 분포 / 카이제곱 분포 / F 분포 / 베타 분포 / 감마 분포 / 디리클레 분포에 대해 설명해주세요. 혹시 연관된 분포가 있다면 연관 관계를 설명해주세요
- 조건부 확률은 무엇일까요?
- 공분산과 상관계수는 무엇일까요? 수식과 함께 표현해주세요
- 신뢰 구간의 정의는 무엇인가요?
- p-value를 고객에게는 뭐라고 설명하는게 이해하기 편할까요?
- p-value는 요즘 시대에도 여전히 유효할까요? 언제 p-value가 실제를 호도하는 경향이 있을까요?
- A/B Test 등 현상 분석 및 실험 설계 상 통계적으로 유의미함의 여부를 결정하기 위한 방법에는 어떤 것이 있을까요?
- R square의 의미는 무엇인가요? 
- 평균(mean)과 중앙값(median)중에 어떤 케이스에서 뭐를 써야할까요?
- 중심극한정리는 왜 유용한걸까요?
- 엔트로피(entropy)에 대해 설명해주세요. 가능하면 Information Gain도요.
- 요즘같은 빅데이터(?)시대에는 정규성 테스트가 의미 없다는 주장이 있습니다. 맞을까요?
- 어떨 때 모수적 방법론을 쓸 수 있고, 어떨 때 비모수적 방법론을 쓸 수 있나요?
- “likelihood”와 “probability”의 차이는 무엇일까요?
- 통계에서 사용되는 bootstrap의 의미는 무엇인가요.
- 모수가 매우 적은 (수십개 이하) 케이스의 경우 어떤 방식으로 예측 모델을 수립할 수 있을까요?
- 베이지안과 프리퀀티스트간의 입장차이를 설명해주실 수 있나요?
- 검정력(statistical power)은 무엇일까요?
- missing value가 있을 경우 채워야 할까요? 그 이유는 무엇인가요?
- 아웃라이어의 판단하는 기준은 무엇인가요?
- 콜센터 통화 지속 시간에 대한 데이터가 존재합니다. 이 데이터를 코드화하고 분석하는 방법에 대한 계획을 세워주세요. 이 기간의 분포가 어떻게 보일지에 대한 시나리오를 설명해주세요
- 출장을 위해 비행기를 타려고 합니다. 당신은 우산을 가져가야 하는지 알고 싶어 출장지에 사는 친구 3명에게 무작위로 전화를 하고 비가 오는 경우를 독립적으로 질문해주세요. 각 친구는 2/3로 진실을 말하고 1/3으로 거짓을 말합니다. 3명의 친구가 모두 "그렇습니다. 비가 내리고 있습니다"라고 말했습니다. 실제로 비가 내릴 확률은 얼마입니까?
- 필요한 표본의 크기를 어떻게 계산합니까?
- Bias를 통제하는 방법은 무엇입니까?
- 로그 함수는 어떤 경우 유용합니까? 사례를 들어 설명해주세요

##### [목차로 이동](#contents)

## 분석 일반
- 좋은 feature란 무엇인가요. 이 feature의 성능을 판단하기 위한 방법에는 어떤 것이 있나요?
- "상관관계는 인과관계를 의미하지 않는다"라는 말이 있습니다. 설명해주실 수 있나요?
- A/B 테스트의 장점과 단점, 그리고 단점의 경우 이를 해결하기 위한 방안에는 어떤 것이 있나요?
- 각 고객의 웹 행동에 대하여 실시간으로 상호작용이 가능하다고 할 때에, 이에 적용 가능한 고객 행동 및 모델에 관한 이론을 알아봅시다.
- 고객이 원하는 예측모형을 두가지 종류로 만들었다. 하나는 예측력이 뛰어나지만 왜 그렇게 예측했는지를 설명하기 어려운 random forest 모형이고, 또다른 하나는 예측력은 다소 떨어지나 명확하게 왜 그런지를 설명할 수 있는 sequential bayesian 모형입니다.고객에게 어떤 모형을 추천하겠습니까?
- 고객이 내일 어떤 상품을 구매할지 예측하는 모형을 만들어야 한다면 어떤 기법(예: SVM, Random Forest, logistic regression 등)을 사용할 것인지 정하고 이를 통계와 기계학습 지식이 전무한 실무자에게 설명해봅시다.
- 나만의 feature selection 방식을 설명해봅시다.
- 데이터 간의 유사도를 계산할 때, feature의 수가 많다면(예: 100개 이상), 이러한 high-dimensional clustering을 어떻게 풀어야할까요?

##### [목차로 이동](#contents)

## 머신러닝
- Cross Validation은 무엇이고 어떻게 해야하나요?
  <details markdown="1">
  <summary>[답안]</summary>

    > 가지고 있는 전체 data를 나누어서 형성한 각 batch가 모두 한번씩은 validation set으로 활용되도록 각 iteration 마다 validation set을 번갈아 바꿔 사용한다.  
    > 주로 Train 데이터가 적을 때 사용한다.
  </details>
  
- 회귀 / 분류시 알맞은 metric은 무엇일까요?
  <details markdown="1">
  <summary>[답안]</summary>

  > **회귀** : `MSE` -> Ground Truth와 Pred. 값의 차이(continuous)를 나타낼 수 있는 metric  
  > **분류** : `Precision`, `Recall`, `Accuracy` -> 분류 결과(discrete)의 신뢰도를 나타낼 수 있는 통계 metric
  </details>
  
- 알고 있는 metric에 대해 설명해주세요(ex. RMSE, MAE, recall, precision ...)
  <details markdown="1">
  <summary>[답안]</summary>

  > ![파일_001](https://user-images.githubusercontent.com/12293076/66627396-8d07d280-ec36-11e9-8f93-6bd0cad570f0.png)
  </details>
- 정규화를 왜 해야할까요? 정규화의 방법은 무엇이 있나요?
  <details markdown="1">
  <summary>[답안]</summary>

  > 데이터를 mini-batch로 쪼개거나, 네트워크의 깊이가 깊어짐에 따라 각 요소들의 분포값이 의도와 달라져서 학습에 차질이 생기는 경우가 발생하기 때문.  
  > **Batch Normalization** : 각 training batch를 미리 정해둔 평균, 분산값으로 정규화한다.  
  > **Weight Normalization** : 동일한 층에 위치한 뉴런들이 가진 weight를 Normalize한다.  

  </details>
- Local Minima와 Global Minima에 대해 설명해주세요.
  <details markdown="1">
  <summary>[답안]</summary>

  > Global minima는 Gradient Descent 방법으로 학습시에, 해당 도메인에 대해 가장 낮은 cost를 가질 수 있는 weight가 위치한 지점이다.  
  > Local Minima는 Gradient Descent로 Global Minima를 찾아가는 과정에서 주변에 지역적으로 Gradient 하강, 상승 구간이 존재하여 빠질 수 있는 가짜 Minima이다.

  </details>
- 차원의 저주에 대해 설명해주세요
  <details markdown="1">
  <summary>[답안]</summary>

  > 차원의 저주는 한 샘플을 특정짓기 위해 많은 정보(다양한 차원의)를 수집할수록 오히려 학습이 어려워짐을 말한다.

  </details>
- Dimension reduction기법으로 보통 어떤 것들이 있나요?
  <details markdown="1">
  <summary>[답안]</summary>

  > ### 기본 방법 : Projection, Manifold 
  > - Projection  
  >   일반적으로, 고차원 데이터의 어떤 특성은 큰 변화가 없고, 어떤 특성은 다른 특성과 연관하여 크게 변하곤 한다. 이를 '데이터가 고차원 공간에서 저차원 subspace(부분 공간)에 위치한다' 라고 하는데, 이것은 고차원 데이터의 특성 중, 일부로 해당 데이터를 표현할 수 있음을 의미한다. 이때, 고차원 데이터를 저차원 subspace로 투영하여 차원을 줄인다.
  > - Manifold  
  >   데이터가 국소적으로는 유클리드 공간에 있으나, 대역적으로는 다른 위상을 가지는 경우 manifold 공간에 있다고 할 수 있다. 고차원 데이터가 실제로는 저차원 manifold에 있다고 가정하고 학습을 진행하여 문제 난이도를 낮출 수 있으나 항상 성공적이지 않기에 가정 전에 데이터를 살펴보는 주의가 필요하다.
  > _[참조: https://excelsior-cjh.tistory.com/167](https://excelsior-cjh.tistory.com/167)_
  > ### 심화 방법: PCA, NMF, LDA  

  </details>
- PCA는 차원 축소 기법이면서, 데이터 압축 기법이기도 하고, 노이즈 제거기법이기도 합니다. 왜 그런지 설명해주실 수 있나요?
  <details markdown="1">
  <summary>[답안]</summary>

  > PCA는 분산이 최대가 되도록 하는 축을 찾고, 그 축과 직교하면서 분산이 최대가 되도록 하는 축을 이어 찾아나가는 방식으로 데이터를 간단히 표현합니다. 이 과정에서 차원이 축소되며, 투영 변환을 반대로 수행하면 데이터의 복원이 가능하고, PCA로 찾은 축들 중, 분산이 적은 축들을 제거함으로서 노이즈를 줄일 수 있습니다.

  </details>
- LSA, LDA, SVD 등의 약자들이 어떤 뜻이고 서로 어떤 관계를 가지는지 설명할 수 있나요?
  <details markdown="1">
  <summary>[답안]</summary>

  > LSA(Latent Semantic Analysis, 잠재의미분석) : SVD를 이용하여 차원을 축소  
  > LDA(Latent Dirichlet Allocation, 잠재디리클레할당) : 주제별 단어분포와 문서별 주제 분포를 학습하여 Topic Modeling 하는 방법  
  > SVD(Singular Value Decomposition) : 특이값 분해  
  >
  > LSA와 SVD를 이용하여 데이터를 분해하고, 함축된 의미(Latent Semantic)를 추출하여 이를 통해 데이터를 압축하거나 차원을 축소한다. 특징 출현 횟수를 사용하는 LDA의 구조에 확률 모델을 도입하여 특징 출현 확률 기반으로 설계된 pLSA(Probabilistic Latent Semantic Analysis)는 SVD대신 NMF를 이용하였다. 이후, LDA는 주제별 단어분포와 문서별 주제분포 모두 고려하되, 디리클레 분포를 따르도록 설계하여 주제를 뽑아낸다.  
  >
  > _[참조: https://bab2min.tistory.com/585](https://bab2min.tistory.com/585)_

  </details>
- Markov Chain을 고등학생에게 설명하려면 어떤 방식이 제일 좋을까요?
  <details markdown="1">
  <summary>[답안]</summary>

  > Markov Chain에서 가장 중요한 개념은 n회의 상태가 n-1회의 상태에만 영향을 받는다는 가정이다.  
  > 따라서 독립 시행으로 일어나는 정사면체 주사위, 정육면체 주사위, 동전 던지기 게임을 상태(state)로, 해당 게임의 결과에 따라 다른 게임으로 종목을 바꾸는 것을 전이(transition) 생각할 수 있다. 전이 조건을 정하고, 특정한 순서대로 게임을 하게 될 확률을 계산해보는 것으로 설명할 수 있을 것이다.

  </details>
- 텍스트 더미에서 주제를 추출해야 합니다. 어떤 방식으로 접근해 나가시겠나요?
  <details markdown="1">
  <summary>[답안]</summary>

  > 간단히 unigram, bigram, TF-IDF로 키워드를 추출하거나 LDA를 이용한다.

  </details>
- SVM은 왜 반대로 차원을 확장시키는 방식으로 동작할까요? 거기서 어떤 장점이 발생했나요?
  <details markdown="1">
  <summary>[답안]</summary>

  > SVM을 비선형 분류 모델로 사용하기 위해 저차원 공간의 데이터를 고차원 공간으로 매핑하여 선형 분리가 가능한 데이터로 변환하여 처리한다.  
  >
  > _[참조: https://ratsgo.github.io/machine%20learning/2017/05/30/SVM3/](https://ratsgo.github.io/machine%20learning/2017/05/30/SVM3/)

  </details>
- 다른 좋은 머신 러닝 대비, 오래된 기법인 나이브 베이즈(naive bayes)의 장점을 옹호해보세요.
  <details markdown="1">
  <summary>[답안]</summary>

  > 1. 결과 도출을 위해 조건부 확률만 계산하면 되므로 매우 빠르고, 메모리를 많이 차지하지 않는다.  
  > 2. 데이터가의 특징들이 서로 독립되어 있을 때 좋은 결과를 얻을 수 있다.
  > 3. 데이터의 양이 적더라도 학습이 용이하다.  
  
  </details>
- Association Rule의 Support, Confidence, Lift에 대해 설명해주세요.
  <details markdown="1">
  <summary>[답안]</summary>

  > ![](https://www.dropbox.com/s/utsfodbxj9uihv5/%ED%8C%8C%EC%9D%BC%202019.%2010.%2021.%20%EC%98%A4%EC%A0%84%2010%2054%2032.jpeg?raw=1)
  >  
  > * Support : X와 Y 두 item이 얼마나 자주 발생하는지를 의미  
  > * Confidence : X가 발생했을 떄, Y도 포함되어 있는 비율  
  > * Lift : X가 발생하지 않았을 때의 Y 발생 비율과 X가 발생했을 때 Y 발생 비율의 대비. 숫자가 1보다 크거나 작은 정도에 따라 연관성을 파악할 수 있다.  
  >  
  > _[참조 : https://rfriend.tistory.com/191](https://rfriend.tistory.com/191)_

  </details>
- 최적화 기법중 Newton’s Method와 Gradient Descent 방법에 대해 알고 있나요?
  <details markdown="1">
  <summary>[답안]</summary>

  > Newton's Method는 연속적이고 미분 가능한 함수에 대해 무작위 값을 대입해서 결과값의 변화 추이를 통해 원하는 해를 근사하는 방법이다. Gradient Descent 방법은 함수의 극대, 극소를 찾는 방법이며, 마찬가지로 지점에서의 미분값에 따라 대입할 값을 갱신하므로 그 근본적인 방법은 동일하다.

  </details>
- 머신러닝(machine)적 접근방법과 통계(statistics)적 접근방법의 둘간에 차이에 대한 견해가 있나요?
  <details markdown="1">
  <summary>[답안]</summary>

  > [주관적 견해] 머신러닝과 통계적 접근방법은 서로 교집합이 큰 종목이다. 이 두 방법의 가장 큰 차이점은 문제 해결 방법의 수립 과정이라고 생각한다. 전자의 경우 머신이 스스로 학습하여 결정하며, 후자의 경우엔 사람이 직접 해결 방법을 설정한다. 통계적으로 사람이 직접 해결 방법을 명확히하기 어려운 문제에 대해 머신러닝 모델을 활용할 수 있다. 하지만 모델 설계 이외에는 제한적으로 사람이 개입되는 머신러닝의 특성상, 기본적으로 오류 가능성을 내포하고 있으며 복잡한 모델일수록 오류가 발생할 수 있는 지점에 대한 예측이 어렵다. 오류 상황에 대한 대응이 필수적인 문제의 경우에는 통계적 접근 방법을 이용하는 것이 더 유리할 수 있다.

  </details>
- 인공신경망(deep learning이전의 전통적인)이 가지는 일반적인 문제점은 무엇일까요?
  <details markdown="1">
  <summary>[답안]</summary>

  > Parameter updating 전략과 각종 Normalization의 미비로 인해 step에 따른 Gradient의 움직임을 효과적으로 제어하기 힘들었기 때문에 모델이 깊게 설계할 수 없었다. 따라서 머신러닝을 사용해야하는 문제는 주로 비선형성이 큰 경우가 많았기 때문에, 얕은 구조의 인공신경망 모델로는 성공적인 대응을 할 수 없는 문제가 있었다.  

  </details>
- 지금 나오고 있는 deep learning 계열의 혁신의 근간은 무엇이라고 생각하시나요?
  <details markdown="1">
  <summary>[답안]</summary>

  > [주관적 견해] 여러 Gradient Descent Optimizer들과 Normalization 기법들이 등장하고, GPU를 통한 부동소수점 연산의 효율화로 인해 Matrix 연산이 빨라지면서 사실상 중단되었던 인공신경망 분야가 한걸음 더 전진한 것이라고 생각한다.  

  </details>
- ROC 커브에 대해 설명해주실 수 있으신가요?
  <details markdown="1">
  <summary>[답안]</summary>

  > ROC 커브는 FPR(False Positive Rate)와 TPR(True Positive rate)을 각각 x축과 y축으로 놓은 그래프이다. FPR은 GT(Grount Truth)가 거짓인 케이스에 대해 참이라고 잘못 예측한 비율이며, TPR은 GT가 참인 케이스에 대해 참이라고 잘 예측한 비율이다. 따라서 전자는 작을수록 성능이 좋고, 후자는 클수록 성능이 좋다.  
  > 두 Parameter에 대해 성능이 좋을수록 그래프 좌상단의 꼭짓점으로 다가가게 되며, 그래프 하단의 면적이 넓어지는 형태를 가지고 있다. 
  >  
  > ![](https://www.dropbox.com/s/9izjybk7aawtof0/ROC%20curve.png?raw=1)

  </details>
- 여러분이 서버를 100대 가지고 있습니다. 이때 인공신경망보다 Random Forest를 써야하는 이유는 뭘까요?
  <details markdown="1">
  <summary>[답안]</summary>

  > 일반적으로 각 단계별로 의존적인 end-to-end 인공신경망과 달리 Random Forest는 여러개의 독립적인 decision tree를 이용하여 결과를 투표하는 형식이므로 다수의 서버에서 병렬적인 처리가 용이하다.

  </details>
- K-means의 대표적 의미론적 단점은 무엇인가요? (계산량 많다는것 말고)
  <details markdown="1">
  <summary>[답안]</summary>

  > K-means 알고리즘은 K개의 Cluster를 미리 입력받아 Expectation과 Maximization 단계를 거듭하여 군집화를 수행한다. 단점들은 다음과 같다.  
  > 1. 초기에 사용자가 해당 데이터에 몇개의 Cluster가 있는지 알고 있어야 한다.  
  > 2. K개의 Cluster를 초기화하는 방법에 따라 결과에 큰 차이가 있다.  
  > 3. 클러스터의 모양이 구형이 아닌 경우, 적용이 힘들다.
  > 4. Maximization단계에서 각 데이터들과의 거리를 기준으로 Cluster 중심점의 위치를 업데이트하므로 데이터의 노이즈에 민감하게 반응한다.  

  </details>
- L1, L2 정규화에 대해 설명해주세요
  <details markdown="1">
  <summary>[답안]</summary>

  > L1, L2 정규화는 모델 학습시에 값이 너무 큰 파라메터의 영향력을 줄이는 전략이다. L1 정규화는 Cost Function에 가중치의 크기(절대값)를 더해주고, L2 정규화는 가중치 크기의 제곱을 더해줌으로써 가중치가 너무 크지 않은 방향으로 학습을 유도한다. 이것은 학습된 모델의 범용성을 높이는 효과를 준다.
  >  
  > _[참조 : https://light-tree.tistory.com/125](https://light-tree.tistory.com/125)_  

  </details>
  
- XGBoost을 아시나요? 왜 이 모델이 캐글에서 유명할까요?
  <details markdown="1">
  <summary>[답안]</summary>

  > XGBoost는 머신러닝에서 각 Feature의 중요도를 계산할 수 있는 툴이다. 학습과 분류가 빠르고 캐글의 머신러닝 경연 우승자 중, 다수가 이 툴을 사용하여 주목받았다. 
  >  
  > _[참조 : https://brunch.co.kr/@snobberys/137](https://brunch.co.kr/@snobberys/137)_  

  </details>

- 앙상블 방법엔 어떤 것들이 있나요?
- SVM은 왜 좋을까요?
- feature vector란 무엇일까요?
- 좋은 모델의 정의는 무엇일까요?
- 50개의 작은 의사결정 나무는 큰 의사결정 나무보다 괜찮을까요? 왜 그렇게 생각하나요?
- 스팸 필터에 로지스틱 리그레션을 많이 사용하는 이유는 무엇일까요?
- OLS(ordinary least squre) regression의 공식은 무엇인가요?


##### [목차로 이동](#contents)

## 딥러닝
## 딥러닝 일반
- 딥러닝은 무엇인가요? 딥러닝과 머신러닝의 차이는?
- 왜 갑자기 딥러닝이 부흥했을까요?
- 마지막으로 읽은 논문은 무엇인가요? 설명해주세요
- Cost Function과 Activation Function은 무엇인가요?
- Tensorflow, Keras, PyTorch, Caffe, Mxnet 중 선호하는 프레임워크와 그 이유는 무엇인가요?
- Data Normalization은 무엇이고 왜 필요한가요?
- 알고있는 Activation Function에 대해 알려주세요. (Sigmoid, ReLU, LeakyReLU, Tanh 등)
- 오버피팅일 경우 어떻게 대처해야 할까요?
- 하이퍼 파라미터는 무엇인가요?
- Weight Initialization 방법에 대해 말해주세요. 그리고 무엇을 많이 사용하나요?
- 볼츠만 머신은 무엇인가요?
- 요즘 Sigmoid 보다 ReLU를 많이 쓰는데 그 이유는?
	- Non-Linearity라는 말의 의미와 그 필요성은?
	- ReLU로 어떻게 곡선 함수를 근사하나?
	- ReLU의 문제점은?
	- Bias는 왜 있는걸까?
- Gradient Descent에 대해서 쉽게 설명한다면?
	- 왜 꼭 Gradient를 써야 할까? 그 그래프에서 가로축과 세로축 각각은 무엇인가? 실제 상황에서는 그 그래프가 어떻게 그려질까?
	- GD 중에 때때로 Loss가 증가하는 이유는?
	- 중학생이 이해할 수 있게 더 쉽게 설명 한다면?
	- Back Propagation에 대해서 쉽게 설명 한다면?
- Local Minima 문제에도 불구하고 딥러닝이 잘 되는 이유는?
	- GD가 Local Minima 문제를 피하는 방법은?
	- 찾은 해가 Global Minimum인지 아닌지 알 수 있는 방법은?
- Training 세트와 Test 세트를 분리하는 이유는?
	- Validation 세트가 따로 있는 이유는?
	- Test 세트가 오염되었다는 말의 뜻은?
	- Regularization이란 무엇인가?
- Batch Normalization의 효과는?
	- Dropout의 효과는?
	- BN 적용해서 학습 이후 실제 사용시에 주의할 점은? 코드로는?
	- GAN에서 Generator 쪽에도 BN을 적용해도 될까?
- SGD, RMSprop, Adam에 대해서 아는대로 설명한다면?
	- SGD에서 Stochastic의 의미는?
	- 미니배치를 작게 할때의 장단점은?
	- 모멘텀의 수식을 적어 본다면?
- 간단한 MNIST 분류기를 MLP+CPU 버전으로 numpy로 만든다면 몇줄일까?
	- 어느 정도 돌아가는 녀석을 작성하기까지 몇시간 정도 걸릴까?
	- Back Propagation은 몇줄인가?
	- CNN으로 바꾼다면 얼마나 추가될까?
- 간단한 MNIST 분류기를 TF, Keras, PyTorch 등으로 작성하는데 몇시간이 필요한가?
	- CNN이 아닌 MLP로 해도 잘 될까?
	- 마지막 레이어 부분에 대해서 설명 한다면?
	- 학습은 BCE loss로 하되 상황을 MSE loss로 보고 싶다면?
	- 만약 한글 (인쇄물) OCR을 만든다면 데이터 수집은 어떻게 할 수 있을까?
- 딥러닝할 때 GPU를 쓰면 좋은 이유는?
	- 학습 중인데 GPU를 100% 사용하지 않고 있다. 이유는?
	- GPU를 두개 다 쓰고 싶다. 방법은?
	- 학습시 필요한 GPU 메모리는 어떻게 계산하는가?
- TF, Keras, PyTorch 등을 사용할 때 디버깅 노하우는?
- 뉴럴넷의 가장 큰 단점은 무엇인가? 이를 위해 나온 One-Shot Learning은 무엇인가? 

##### [목차로 이동](#contents)

## 컴퓨터 비전
- OpenCV 라이브러리만을 사용해서 이미지 뷰어(Crop, 흑백화, Zoom 등의 기능 포함)를 만들어주세요
- 딥러닝 발달 이전에 사물을 Detect할 때 자주 사용하던 방법은 무엇인가요?
- Fatser R-CNN의 장점과 단점은 무엇인가요?
- dlib은 무엇인가요?
- YOLO의 장점과 단점은 무엇인가요?
- 제일 좋아하는 Object Detection 알고리즘에 대해 설명하고 그 알고리즘의 장단점에 대해 알려주세요
	- 그 이후에 나온 더 좋은 알고리즘은 무엇인가요? 
- Average Pooling과 Max Pooling의 차이점은?
- Deep한 네트워크가 좋은 것일까요? 언제까지 좋을까요?
- Residual Network는 왜 잘될까요? Ensemble과 관련되어 있을까요?
- CAM(Class Activation Map)은 무엇인가요?
- Localization은 무엇일까요?
- 자율주행 자동차의 원리는 무엇일까요?
- Semantic Segmentation은 무엇인가요?
- Visual Q&A는 무엇인가요?
- Image Captioning은 무엇인가요?
- Fully Connected Layer의 기능은 무엇인가요?
- Neural Style은 어떻게 진행될까요?
- CNN에 대해서 아는대로 얘기하라
	- CNN이 MLP보다 좋은 이유는?
	- 어떤 CNN의 파라미터 개수를 계산해 본다면?
	- 주어진 CNN과 똑같은 MLP를 만들 수 있나?
	- 풀링시에 만약 Max를 사용한다면 그 이유는?
	- 시퀀스 데이터에 CNN을 적용하는 것이 가능할까?

##### [목차로 이동](#contents)

## 자연어 처리
- One Hot 인코딩에 대해 설명해주세요
- POS 태깅은 무엇인가요? 가장 간단하게 POS tagger를 만드는 방법은 무엇일까요?
- 문장에서 "Apple"이란 단어가 과일인지 회사인지 식별하는 모델을 어떻게 훈련시킬 수 있을까요?
- 뉴스 기사에 인용된 텍스트의 모든 항목을 어떻게 찾을까요?
- 음성 인식 시스템에서 생성된 텍스트를 자동으로 수정하는 시스템을 어떻게 구축할까요?
- 잠재론적, 의미론적 색인은 무엇이고 어떻게 적용할 수 있을까요?
- 영어 텍스트를 다른 언어로 번역할 시스템을 어떻게 구축해야 할까요?
- 뉴스 기사를 주제별로 자동 분류하는 시스템을 어떻게 구축할까요?
- Stop Words는 무엇일까요? 이것을 왜 제거해야 하나요?
- 영화 리뷰가 긍정적인지 부정적인지 예측하기 위해 모델을 어떻게 설계하시겠나요?
- TF-IDF 점수는 무엇이며 어떤 경우 유용한가요?
- 한국어에서 많이 사용되는 사전은 무엇인가요?
- Regular grammar는 무엇인가요? regular expression과 무슨 차이가 있나요?
- RNN에 대해 설명해주세요
- LSTM은 왜 유용한가요?
- Translate 과정 Flow에 대해 설명해주세요
- n-gram은 무엇일까요?
- PageRank 알고리즘은 어떻게 작동하나요?
- depedency parsing란 무엇인가요?
- Word2Vec의 원리는?
	- 그 그림에서 왼쪽 파라메터들을 임베딩으로 쓰는 이유는?
	- 그 그림에서 오른쪽 파라메터들의 의미는 무엇일까?
	- 남자와 여자가 가까울까? 남자와 자동차가 가까울까?
	- 번역을 Unsupervised로 할 수 있을까?

##### [목차로 이동](#contents)

## 강화학습
- MDP는 무엇일까요?
- 가치함수는 무엇일까요? 수식으로도 표현해주세요
- 벨만 방정식은 무엇일까요? 수식으로도 표현해주세요
- 강화학습에서 다이나믹 프로그래밍은 어떤 의미를 가질까요? 한계점은 무엇이 있을까요?
- 몬테카를로 근사는 무엇일까요? 가치함수를 추정할 때 어떻게 사용할까요?
- Value-based Reinforcement Learning과 Policy based Reinforcement Learning는 무엇이고 어떤 관계를 가질까요?
- 강화학습이 어려운 이유는 무엇일까요? 그것을 어떤 방식으로 해결할 수 있을까요?
- 강화학습을 사용해 테트리스에서 고득점을 얻는 프로그램을 만드려고 합니다. 어떻게 만들어야 할까요?

##### [목차로 이동](#contents)

## GAN
- GAN에 대해 아는대로 설명해주세요
- GAN의 단점은 무엇인가요?
- LSGAN에 대해 설명해주세요
- GAN이 왜 뜨고 있나요?
- Auto Encoder에 대해서 아는대로 얘기하라
	- MNIST AE를 TF나 Keras등으로 만든다면 몇줄일까?
	- MNIST에 대해서 임베딩 차원을 1로 해도 학습이 될까?
	- 임베딩 차원을 늘렸을 때의 장단점은?
	- AE 학습시 항상 Loss를 0으로 만들수 있을까?
	- VAE는 무엇인가?
- 간단한 MNIST DCGAN을 작성한다면 TF 등으로 몇줄 정도 될까? 
	- GAN의 Loss를 적어보면?
	- D를 학습할때 G의 Weight을 고정해야 한다. 방법은?
	- 학습이 잘 안될때 시도해 볼 수 있는 방법들은?

##### [목차로 이동](#contents)

## 추천 시스템
- 추천 시스템에서 사용할 수 있는 거리는 무엇이 있을까요?
- User 베이스 추천 시스템과 Item 베이스 추천 시스템 중 단기간에 빠른 효율을 낼 수 있는 것은 무엇일까요?
- 성능 평가를 위해 어떤 지표를 사용할까요?
- Explicit Feedback과 Implicit Feedback은 무엇일까요? Impicit Feedback을 어떻게 Explicit하게 바꿀 수 있을까요?
- Matrix Factorization은 무엇인가요? 해당 알고리즘의 장점과 단점은?
- SQL으로 조회 기반 Best, 구매 기반 Best, 카테고리별 Best를 구하는 쿼리를 작성해주세요
- 추천 시스템에서 KNN 알고리즘을 활용할 수 있을까요?
- 유저가 10만명, 아이템이 100만개 있습니다. 이 경우 추천 시스템을 어떻게 구성하시겠습니까?
- 딥러닝을 활용한 추천 시스템의 사례를 알려주세요
- 두 추천엔진간의 성능 비교는 어떤 지표와 방법으로 할 수 있을까요? 검색엔진에서 쓰던 방법을 그대로 쓰면 될까요? 안될까요?
- Collaborative Filtering에 대해 설명한다면?
- Cold Start의 경우엔 어떻게 추천해줘야 할까요?
- 고객사들은 기존 추천서비스에 대한 의문이 있습니다. 주로 매출이 실제 오르는가 하는 것인데, 이를 검증하기 위한 방법에는 어떤 것이 있을까요? 위 관점에서 우리 서비스의 성능을 고객에게 명확하게 인지시키기 위한 방법을 생각해봅시다.

##### [목차로 이동](#contents)

--- 
