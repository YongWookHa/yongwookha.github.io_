---
layout: post
title: Pytorch-Bert 구현하기 
subtitle: Series-1) pytorch-template 살펴보기
tags: [MACHINE_LEARNING, NLP, PYTORCH]
image: /img/NLP-image.jpg
comments: true
---

요즘 Deep Learning은 Tensorflow와 Pytorch 두 진영으로 나뉘는 느낌이다. 초창기에는 CAFFE 커뮤니티가 꽤나 크게 조성되어 있었던 느낌이었는데, 확실히 변화가 있었던 듯 하다. 그동안 구글의 힘을 업고 Tensorflow가 크게 성장했지만 편리성을 앞세운 Theano와 Pytorch도 만만치 않았다. Facebook에서 Pytorch를 선택하고 Tensorflow는 Keras를 품으면서 최근 Github에 올라오는 신규 논문 모델 구현 중, 대부분은 Tensorflow 혹은 Pytorch인 것 같다. 물론 Tensorflow 모델을 Pytorch에 import하거나 그 반대도 가능하기 때문에 크게 의미 없는 이야길지도 모르겠다.

내가 알기로, 구글에서 발표하는 논문들은 모두 TF로 구현되어 올라오고 있다. ML 분야에 많은 재원들이 투입됨에 따라 점차 라이브러리화된 코드들은 오히려 코어 코드를 읽기 힘들게 만드는 경향이 있다. Transformer의 official source가 구현되어 있는 [tensor2tensor](https://github.com/tensorflow/tensor2tensor)가 읽기가 참 까다롭게 느껴지는 녀석들 중 하나다.

대학원때부터 Tensorflow와 Keras만 줄곧 다뤄왔는데도 아직 처음보는 코드를 읽어내는데 시간이 꽤나 걸린다. 취직을 하고나서야 공부하기 시작한 Pytorch는 이러한 불편함이 훨씬 적은 것 같다. Autograd를 기반으로 각 Function들을 잘게 모듈화하기 좋게끔 설계되어 있는 Pytorch를 공부하면서, '왜 진작하지 않았을까' 하는 아쉬움마저 들곤 한다.

[pytorch-template](https://github.com/victoresque/pytorch-template)