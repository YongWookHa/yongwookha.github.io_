---
layout: post
title: BERT모델 Pytorch Template에 옮겨쓰기
subtitle: 1. pytorch-template 살펴보기
tags: [MACHINE_LEARNING, NLP, PYTORCH]
image: /img/NLP-image.jpg
comments: true
---

요즘 Deep Learning에는 Tensorflow의 영향력이 지배적이다. 초창기에는 CAFFE 커뮤니티가 꽤나 크게 조성되어 있었던 느낌이었는데, 확실히 변화가 있었던 듯 하다. 편리성을 앞세운 Theano와 Pytorch의 성장도 구글의 힘을 등에 업고 Keras까지 품어버린 Tensorflow의 독주를 막을 수 없는 모양이다. 그나마 Pytorch가 Facebook의 선택을 받고 커뮤니티를 활발히 유지하면서 대항중이다. 최근 Github에 올라오는 신규 논문 모델 구현 중, 대부분은 Tensorflow를 썼다. 간혹가다 Pytorch가 끼어있는게 보인다. 실제로 통계자료를 보니, Tensorflow와 Keras의 비중이 어마어마하다. 

![](https://miro.medium.com/max/1200/1*s_BwkYxpGv34vjOHi8tDzg.png)

내가 알기로, 구글에서 발표하는 논문들은 모두 TF로 구현되어 올라오고 있다. ML 분야에 많은 재원들이 투입됨에 따라 점차 라이브러리화된 코드들은 오히려 코어 코드를 읽기 힘들게 만드는 경향이 있다. 라이브러리의 규모가 커지면서 다른 기능들이 추가되면서 코드가 복잡해지기 때문이다. Transformer의 official source가 구현되어 있는 [tensor2tensor](https://github.com/tensorflow/tensor2tensor)가 읽기가 참 까다롭게 느껴지는 녀석들 중 하나다. tensor2tensor는 Transformer 뿐만아니라 다양한 Deep Learning 모델들이 쉽게 사용될 수 있도록 만들어져있다.

대학원때부터 Tensorflow와 Keras만 줄곧 다뤄왔는데도 아직 처음보는 코드를 읽어내는데 시간이 꽤나 걸린다. 이와 다르게 취직을 하고나서야 공부하기 시작한 Pytorch는 이러한 불편함이 훨씬 적은 것 같다. Autograd를 기반으로 각 Function들을 잘게 모듈화하기 좋게끔 설계되어 있는 Pytorch를 공부하면서, '왜 진작하지 않았을까' 하는 아쉬움마저 들곤 한다.

Pytorch 숙련도를 올리고 NLP 모델들을 더 자세히 공부하기 위해서 [pytorch-template](https://github.com/victoresque/pytorch-template)에서 받을 수 있는 템플릿에다가 [Pytorchic BERT](https://github.com/dhlee347/pytorchic-bert)을 옮겨 구현하는 작업을 해보려고 한다. Google의 [BERT](https://github.com/google-research/bert)의 Tensorflow code를 Pytorch로 re-implementation한 [이동현님](https://github.com/dhlee347)의 repository이다. 코드가 읽기 좋게 깔끔해서 베이스 코드로 삼게 되었다.

이왕이면 `ctrl+c`, `ctrl+v`를 최소한으로 이용하면서 코드를 찬찬히 음미할 수 있도록 하려고 한다. 회사일에 치여있다보면 언제쯤 마무리될지 모르겠지만, 시작이 반이니 이렇게 블로그에 포스트 하나를 남기는걸로 만천하에 공언해버릴 속셈이다. 원래 토이 프로젝트든, 다이어트든, 할거라고 말해놓는게 첫걸음이니 말이다.

Template의 Directory structure는 다음과 같이 생겼다.

```
pytorch-template/
│
├── train.py - main script to start training
├── test.py - evaluation of trained model
│
├── config.json - holds configuration for training
├── parse_config.py - class to handle config file and cli options
│
├── new_project.py - initialize new project with template files
│
├── base/ - abstract base classes
│   ├── base_data_loader.py
│   ├── base_model.py
│   └── base_trainer.py
│
├── data_loader/ - anything about data loading goes here
│   └── data_loaders.py
│
├── data/ - default directory for storing input data
│
├── model/ - models, losses, and metrics
│   ├── model.py
│   ├── metric.py
│   └── loss.py
│
├── saved/
│   ├── models/ - trained models are saved here
│   └── log/ - default logdir for tensorboardX and logging output
│
├── trainer/ - trainers
│   └── trainer.py
│
├── logger/ - module for tensorboardX visualization and logging
│   ├── visualization.py
│   ├── logger.py
│   └── logger_config.json
│  
└── utils/ - small utility functions
    ├── util.py
```

모듈들을 클래스로 선언하여 상속을 기반으로 계층적인 구현을 지향하는 Pytorch에 딱 맞는 구조인 것 같다. 각 모듈들의 관계를 머리속에 잘 그릴 수 있어야하기에 집중력을 가지고 진행해야할 것 같다.


