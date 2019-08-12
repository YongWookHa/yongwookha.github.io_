---
layout: post
title: BERT모델 Pytorch Template에 옮겨쓰기
subtitle: 1. pytorch-template 살펴보기
tags: [MACHINE_LEARNING, NLP, PYTORCH]
image: /img/PyTorch-logo.jpg
comments: true
---

요즘 Deep Learning에는 Tensorflow의 영향력이 지배적이다. 초창기에는 CAFFE 커뮤니티가 꽤나 크게 조성되어 있었던 느낌이었는데, 확실히 변화가 있었던 듯 하다. 편리성을 앞세운 Theano와 Pytorch의 성장도 구글의 힘을 등에 업고 Keras까지 품어버린 Tensorflow의 독주를 막을 수 없는 모양이다. 그나마 Pytorch가 Facebook의 선택을 받고 커뮤니티를 활발히 유지하면서 대항중이다. 최근 Github에 올라오는 신규 논문 모델 구현 중, 대부분은 Tensorflow를 썼다. 간혹가다 Pytorch가 끼어있는게 보인다. 실제로 통계자료를 보니, Tensorflow와 Keras의 비중이 어마어마하다. 

![](https://miro.medium.com/max/1200/1*s_BwkYxpGv34vjOHi8tDzg.png)

내가 알기로, 구글에서 발표하는 논문들은 모두 TF로 구현되어 올라오고 있다. ML 분야에 많은 재원들이 투입됨에 따라 점차 라이브러리화된 코드들은 오히려 코어 코드를 읽기 힘들게 만드는 경향이 있다. 라이브러리의 규모가 커지면서 다른 기능들이 추가되면서 코드가 복잡해지기 때문이다. Transformer의 official source가 구현되어 있는 [tensor2tensor](https://github.com/tensorflow/tensor2tensor)가 읽기가 참 까다롭게 느껴지는 녀석들 중 하나다. tensor2tensor는 Transformer 뿐만아니라 다양한 Deep Learning 모델들이 쉽게 사용될 수 있도록 만들어져있다.

대학원때부터 Tensorflow와 Keras만 줄곧 다뤄왔는데도 아직 처음보는 코드를 읽어내는데 시간이 꽤나 걸린다. 이와 다르게 취직을 하고나서야 공부하기 시작한 Pytorch는 이러한 불편함이 훨씬 적은 것 같다. Autograd를 기반으로 각 Function들을 잘게 모듈화하기 좋게끔 설계되어 있는 Pytorch를 공부하면서, '왜 진작하지 않았을까' 하는 아쉬움마저 들곤 한다.

## Pytorch Template for Study

Pytorch 숙련도를 올리고, 동시에 NLP 모델들을 더 자세히 공부하기 위해 작은 프로젝트를 해보려고 한다. [moemen95의 Pytorch-template](https://github.com/moemen95/Pytorch-Project-Template)에서 공유하는 Pytorch 코드 구조화 템플릿에다가 [Pytorchic BERT](https://github.com/dhlee347/pytorchic-bert)을 옮겨 구현해보는 것이다. [Pytorchic BERT](https://github.com/dhlee347/pytorchic-bert)는 Google의 [BERT](https://github.com/google-research/bert)의 Tensorflow 코드를 Pytorch로 re-implementation한 [이동현님](https://github.com/dhlee347)의 repository이다. 코드가 읽기 좋게 깔끔해서 베이스 코드로 삼게 되었다.

이왕이면 `ctrl+c`, `ctrl+v`를 최소한으로 이용하면서 코드를 찬찬히 음미할 수 있도록 하려고 한다. 회사일에 치여있다보면 언제쯤 마무리될지 모르겠지만, 시작이 반이니 이렇게 블로그에 포스트 하나를 남기는걸로 만천하에 공언해버릴 속셈이다. 원래 토이 프로젝트든, 다이어트든, 할거라고 말해놓는게 첫걸음이니 말이다.

Template Structure는 다음과 같이 생겼다.

![](https://github.com/moemen95/Pytorch-Project-Template/raw/master/utils/assets/class_diagram.png)

처음에는 [victoresque의 Pytorch Template](https://github.com/victoresque/pytorch-template)를 사용하려고 했지만 [moemen95의 Pytorch-template](https://github.com/moemen95/Pytorch-Project-Template)의 구조에서 Agent를 중심으로 각 모듈들을 기능적으로 분리 구성하는 부분이 마음에 들어 변경했다. 각 모듈들의 관계를 머리속에 정확히 정리하면서 집중해야겠다.

## +08.10

현재, [codertimo님의 BERT-pytorch](https://github.com/codertimo/BERT-pytorch)와 [이동현님의 Pytorchic BERT](https://github.com/dhlee347/pytorchic-bert)를 모두 참조하여 구현중이다. 두 Repo의 가장 큰 차이는 학습에 사용하는 데이터의 형식이다. 처음에는 그냥 똑같은 코드를 옮기는 수준으로 계획한 프로젝트였지만 점차 괜히 욕심이 난다. 지금은 내 나름대로 정의한 데이터 형식을 따르게끔 Dataloader와 Agent를 조금 수정하고 있다. 많은 시간을 투자하지 못해서 생각보다 조금 길어지는듯 하지만, 다음주 이내에 마무리 해보려 한다.

Github에서 Private Repository로 구현중이지만, 완성하면 Open으로 전환하는 것도 고려해봐야겠다.
