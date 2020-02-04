---
layout: post
title: A Light BERT, ALBERT (2020) 
subtitle: BERT와 비교하여 간단 정리
tags: [MACHINE_LEARNING, NLP]
image: /img/albert-einstein.jpg
comments: true
---

NLP 역량을 갖추겠다고 Transformer와 BERT를 공부하고 직접 구현해보느라고 난리쳤던게 엊그제 같은데, 그것도 벌써 반년이 다 되어간다. 한동안은 다시 Image Processing 분야만 파고 있었더니 NLP쪽 진도가 좀 밀린 것 같다.

이번에는 문서 요약 관련한 R&D를 해야할 일이 생겨서 이참에 관련 논문을 찾아봤다. NLP에서 손을 놓기 직전에 출시한 XLNet의 BERT 능가 소식을 들으며, 저건 사이즈가 너무 커서 실제로 적용하긴 힘들겠다 싶었는데, 그새 개선된 버전들이 많이 나온 듯 하다. 이 중, 가장 인상적인 녀석은 ALBERT인 것 같다. 이름에서부터 BERT의 후속작인게 보여서 기존 BERT 모델과의 차이점과 개선 아이디어 위주로 간단하게 정리해봤다. 

자세한 내용은 [Google AI Blog - ALBERT](https://ai.googleblog.com/2019/12/albert-lite-bert-for-self-supervised.html)에서 참조하시길.

-----------------------------------------

2020년 상반기. 대부분의 NLP Tasks  SOTA 는 **ALBERT** -> ICLR 2020 ACCEPTED

## 1. ALBERT의 성능에는 무엇이 기여하는가?


**IDEA** : BERT는 크다. 그냥 막 쓰기에는 너무 크다. 그래서 이걸 줄여야겠다.  

---  
![ALBERT](https://www.dropbox.com/s/vxa7myiarjj79yt/BERT-vs-ALBERT.png?raw=1)  

---  

### A. Allocate the model's capacity more efficiently  

어디서 무엇을 학습해야하는가에 대한 고민.  

*   Input-Level Embedding  
    문맥에 상관없는 표현(Context Independent)을 학습시키자 -> relatively-low demension(e.g., 128)  
*   Hidden-Layer Embedding  
    문맥과 연관된 표현(Context Dependent)을 학습시키자 -> higer demension(e.g., 768)  

이 아이디어를 통해 Projection Layer의 parameter 수를 80% 감소시켰지만, 성능 손실은 거의 Zero (SQuAD2.0 80.4 -> 80.3  

  

### B. Parameter Sharing

기존의 Transformer 기반 모델들(BERT, XLNet, RoBERTa)는 독립된 Layer들이 쌓여있었음. 그러나 각 층들이 비슷한 연산을 수행하는 것을 확인.
같은 Layer를 쌓아올려서 Parameter 수를 감소시킴. (Attention-feedforward 블록에서 90% 감소 / 전체에 모델에서 70% 감소)
성능 손실은 미미함 (80.3 -> 80.0)

  
### C. Scale up the model AGAIN

기존 BERT-base 모델의 Parameter 수는 110M, ALBERT-base 는 12M
전체 모델의 Parameter 개수가 90% 감소하였기 때문에 Hidden Layer의 Embedding size를 키울 여력이 생김.
Hidden layer를 10~20배 키울 수 있음. 논문의 ALBERT-xxlarge 실험에서는 hidden-size 4096 이용. (BERT-large의 Hidden-size 1024)
BERT-large에 비해 30% 적은 Parameter 수임에도 큰 성능 향상 얻음 (SQuAD 83.9 -> 88.1)




