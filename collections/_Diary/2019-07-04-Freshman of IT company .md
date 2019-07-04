---
layout: post
title: IT 회사의 개발 환경
subtitle : 신입사원 적응기
tags: [Diary, Develop, IT]
comments: true
---

학부와 석사 과정 동안 다양한 개발 환경을 경험하지 않았던 것이 아쉽다. 당시만 해도, 로컬에 공들여 설치해놓은 GPU 드라이버나 CUDA와 같은 NVIDIA dependency들이 잘 동작하는데 굳이 다른 환경으로 벗어날 필요가 거의 없었다. 만약 필요하다 해도 파이썬 모듈 관리 정도의 수준이었기에 Anaconda의 개발환경 격리 기능으로 충분히 커버 가능했던 것도 있다. 하드웨어적으로도, 논문용 모델 실험에서는 로컬에 설치된 GTX1080 하나로 충분했고, 부족하더라도 batch size를 줄이는 걸로 대응할 수 있었다. 그런데 이제 회사에 와서 일을 하려니 실험 장비는 모두 서버에 설치되어 있는데다가 개인 GPU는 없고, 다양한 모델을 다뤄야해서 달라진 개발환경에 적응해야했다. 이 모든걸 해결하기 위해 여러가지 새로운 문물들을 경험하느라 요즘 정신이 없다. 변화된 환경을 나열하자면, 다음과 같다.

* Local -> `SSH`
* MS to-do -> `Trello`
* Kakaotalk -> `Slack`
* Anaconda -> `Docker`
* Pytorch -> `VSCode`

SSH는 Putty와 Windows Command Line에 SSH 기능을 추가하여 사용하고 있다. 카톡으로 업무 공유하는 회사가 제일 안좋다는 이야기를 들었었는데, 다행히 우리도 카톡이 아닌 Slack으로 작업 상황 공유 및 커뮤니케이션을 수행한다. 또 Trello의 Kanban Board를 통해 협업 경험을 쌓고 있다. 

그동안 왜 안쓰고 있었을까 싶은 **제일 좋은** 개발 환경 두 가지는 `VSCode`와 `Docker`이다. `VSCode`는 안되는게 없는 일종의 개발 플랫폼이다. 오픈소스 기반인데다 Extension을 통해 다양한 기능들이 추가되어 있다. Build, Debug도 Python, C, Java 등등 가리는 언어가 없다. 처음에는 그냥 code editor로만 쓰다가 지금은 내가 사용하는 모든 언어의 IDE로 `VSCode`를 쓰고 있다. 심지어 지금 쓰고 있는 Markdown 포스트도 VSCode에서 결과물을 실시간 확인하며 작성중이다. 덕분에 요즘에는 기존에 쓰던 Markdown editor([Typora](https://typora.io/))도 실행해본지 오래됐다. 원격 개발을 위해 SSH Remote도 쓰고 있다. `VSCode`의 Python 원격 개발 환경은 정말이지 너무 편하다. 서버의 Interpreter를 그대로 사용할 수 있기 때문에, 로컬에서 개발하는것 같이 편안하게 쓸 수 있다. Putty로 SSH 연결하고 FileZilla로 파일 관리하는 복잡함이 사라졌다.

<center><img src="https://subicura.com/assets/article_images/2017-01-19-docker-guide-for-beginners-1/docker-logo.png" alt="Docker" width="200"/></center>

그리고 `Docker`. 정말 이렇게 혁신적인 배포 환경을 이제껏 쓰지 않았단 사실이 너무 애석하다. 귀여운 고래 로고는 분명 본적 있지만 입사 후에나 직접 사용해보게 되었다. 매번 신세계를 경험중이다. 처음에는 NLP를 새로 공부하면서 여러 모델에 대한 실험을 위해 딥러닝 서버에 서로 다른 NVIDIA 드라이버 혹은 CUDA 버전이 필요하여 방법을 찾다가 처음 사용하게 되었다. 그때를 시작으로 이제는 DB부터 리눅스까지 안쓰는데가 없다. 아니나다를까 알고보니 이미 회사의 모든 솔루션 배포 환경은 [Jenkins](https://jenkins.io/)와 [Docker](https://www.docker.com/)로 되어있었다. 최근에는 솔루션에 ML을 덧붙이는 작업을 하면서 계속해서 `Docker`와 함께하는 중이다. 듣자하니 구글에서는 지메일부터 유투브까지 모든 process들이 container로 돌아간다고 한다. 조금 늦었지만 나도 이제 주류에 편승해서 하나하나 열심히 배워나가야겠다.

고향을 등지고 떠나온 삭막한 서울의 디지털 산업단지 생활은 회색빛이다. 아직 한평짜리 고시텔에서 저녁마다 작은 노트북 모니터에 갖혀 살고 있다. 조만간 자취방으로 이사하고 여유를 갖추면 토이 프로젝트도 하나 진행해보고 조금 더 진취적인 하루를 보낼 수 있도록 해야겠다.
