---
layout: post
title: Householder transform
subtitle: QR decomposition
tags: [ETC, math, householder, transformation]

---

이번 포스트에서는 Householder transform에 대해 공부한 내용을 정리한다. 

keywords : `QR 분해`, `Householder reflector`

## 먼저 QR 분해에 대해 알아보자.

> wiki: 선형대수학에서, QR 분해는 실수 행렬을 직교행렬과 상삼각행렬의 곱으로 나타내는 행렬분해이다.

<img width="515" alt="파일_001" src="https://user-images.githubusercontent.com/12293076/60099749-41b6da80-9793-11e9-8cea-8c1a0e95d34c.png">

여기서 **상삼각행렬**이 의미를 가진다. size가 큰 행렬의 고윳값(eigen value)를 구하기 위해 [Jacobi rotation](https://en.wikipedia.org/wiki/Jacobi_rotation)을 하는 경우나, 머신러닝을 위해 입력값과 weight 행렬을 곱해줘야하는 상황 등에서 피연산자 행렬을 상삼각행렬로 변환해주면 총 계산량을 획기적으로 줄일 수 있다.

우아한 형제들 기술 블로그에 김세환님이 포스팅하신 [파이썬으로 linear regression 해보기](http://woowabros.github.io/study/2018/08/01/linear_regression_qr.html)에서도 Householder transform으로 QR 분해를 하는 내용이 소개되어 있다. 더 자세한 내용은 [ML wiki](http://mlwiki.org/index.php/Householder_Transformation)에 있으니 참조하도록 하자. 아래 내용은 앞의 두 링크를 참조하여 재작성한 것이다.

<img width="546" alt="파일_001 (1)" src="https://user-images.githubusercontent.com/12293076/60099753-44b1cb00-9793-11e9-9b76-b09f13dd198c.png">

Householder Tranformation을 위해 P를 구한 후 x를 곱해 Px를 구한다. 하지만 프로그래밍 과정에서 중간 과정은 큰 의미가 없다. Householder transformation의 정의에서 Q가**symmetric**하며, **orthogonal**함을 알아 낼 수 있다. 이것을 이용해서 계산과정을 간소화하여 직접적으로 P를 구하지 않고 transform을 수행하게 된다. 아래는 두 성질을 알아내는 과정이다.

<img width="622" alt="파일_001 (3)" src="https://user-images.githubusercontent.com/12293076/60109008-8eef7800-97a4-11e9-8e11-d88cb0d1e4fe.png">


계산과정을 간소화하는 방법에 대한 정보는 공개되어 있는 [Numerical Methods in Engineering With Python 3](https://doc.lagout.org/programmation/python/Numerical%20Methods%20in%20Engineering%20with%20Python%203%20%283rd%20ed.%29%20%5BKiusalaas%202013-01-21%5D.pdf) 교재의 365페이지부터 자세하게 나와 있으니 참조하면 좋을 것 같다.


아래는 QR 분해 결과를 리턴하는 Python 코드.
```python
def qr_householder(A):
    m, n = A.shape
    Q = np.eye(m) # Orthogonal transform so far
    R = A.copy() # Transformed matrix so far

    for j in range(n):
        # Find H = I - beta*u*u' to put zeros below R[j,j]
        x = R[j:, j]
        normx = np.linalg.norm(x)
        rho = -np.sign(x[0])
        u1 = x[0] - rho * normx
        u = x / u1
        u[0] = 1
        beta = -rho * u1 / normx

        R[j:, :] = R[j:, :] - beta * np.outer(u, u).dot(R[j:, :])
        Q[:, j:] = Q[:, j:] - beta * Q[:, j:].dot(np.outer(u, u))
        
    return Q, R
```

아래는 diagonal 성분만 남도록 transform하여 리턴하는 코드.
```python
def householder(a):
    n = len(a)
    for k in range(n-2):
        u = a[k+1:n,k]
        uMag = math.sqrt(np.dot(u,u))
        if u[0] < 0.0: uMag = -uMag
        u[0] = u[0] + uMag
        h = np.dot(u,u)/2.0
        v = np.dot(a[k+1:n,k+1:n],u)/h
        g = np.dot(u,v)/(2.0*h)
        v = v - g*u
        a[k+1:n,k+1:n] = a[k+1:n,k+1:n] - np.outer(v,u) -np.outer(u,v)
        a[k,k+1] = -uMag
    return np.array(a)
```
