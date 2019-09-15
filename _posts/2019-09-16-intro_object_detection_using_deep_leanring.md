---
layout: post
title: "[Intro] Object Detection using Deep Learning"
tag: [Deep Learning, Object Detection, Image Classification, Computer vision]
date: 2019-09-16
description: "딥러닝 기반의 Object Detection 모델들을 공부하기 전에 Image Classification과 기존 컴퓨터 비전에서의 문제 해결 방법들에 대해 알아보자."
categories: [Deep Learning, Object Detection]
---

## [Intro] Object Detection using Deep Learning



### 딥러닝 이전의 Computer Vision

---

딥러닝 이전의 컴퓨터 비전 분야에서는 영상 속에서 물체를 찾기 위해 우선적으로 찾고자 하는 물체의 특징을 추출해 내고, 영상 속에서 그 특징을 갖는 혹은 유사한 영역을 찾아냄으로써 연구를 진행해왔다. 특징 추출(feature extraction) 방법의 예시로는 간단하게 색상 정보를 이용하는 color histogram 이나, 형태 정보를 이용하는 canny edge detection, HoG feature 부터, 크기 정보까지 고려하는 SIFT, SURF 등등 물체의 특징이 될 만한 정보들을 개발자들이 직접 설계하며 다양하게 만들어져 왔다. 하지만 이 방법들은 범용적으로 쓰이기에 적합한 특징점들은 아니며 특정 물체를 구별하기 위해서는 문제지와 정답지같은 데이터로 학습이 필요하고 이를 위해 SVM, Decision Tree 등과 같은 machine learning 기법이 사용되곤 한다.



### 2012년 ImageNet과 Image Classification

---

ImageNet이라는 영상 데이터 베이스를 기반으로 한 1000가지 물체 분류 대회인 ILSVRC(ImageNet Large Scale Visual Recognition Challenge)가 있었고 2012년도에 캐나다의 토론토 대학이 우승을 차지하였다. 이 때 사용된 기법이 딥러닝을 이용한 CNN(Convolutional Neural Network)이었고 모델 이름은 바로 그 유명한 AlexNet이다.

![ImageNet_AlexNet2](https://user-images.githubusercontent.com/2151950/64918059-da2d8b80-d7d3-11e9-9d8e-6be72698f2de.png)

AlexNet이 나타나기 전과 후 대회의 성능 지표인 인식 오류 비율이 상당히 줄어든 것을 볼 수 있으며 2012년 전과 후의 알고리즘의 큰 차이점은 바로 딥러닝 기반인가 아닌가이다. 2012년 이전 알고리즘은 개발자들이 직접 물체의 특징이 될 만한 점을 설계하고 추출하였지만 그 후로는 딥러닝 모델들이 주어진 문제지와 정답지 데이터(ex, 강아지가 존재하는 사진과 해당 사진은 강아지라는 정답지)로부터 스스로 특징이 될 만한 점을 추출하고 학습하였다. 이러한 딥러닝 모델이 커지고 발전하면서 2015년에 이르러 사람보다 더 정확한 성능을 가지게 되었다.

(엄밀하게 말하자면 ImageNet 데이터 셋에 대해 사람보다 잘 인식한다는 것이지 모든 물체를 사람보다 잘 인식한다는 말은 절대 아니다 ㅎ)



### Image Classification 에서 Object Detection 으로

---

이렇게 사람이 직접 물체의 특징점을 설계하는 것보다 딥러닝이 스스로 학습하며 설계한 특징점이 더 정확하다는 것을 결과론적으로 확인하게 되면서 더 범용적이고 효율적인 특징점을 설계하는 것이 아닌 그러한 특징점을 설계할 수 있는 세로운 딥러닝 모델을 설계하는 방향으로 패러다임이 변화하게 되었다. 동시에 문제도 단순히 한 장의 사진이 어떤 물체인지 판별하는 Image Classification에서 여러 물체가 존재하는 한장의 사진 속에서 각각의 위치를 찾아내고 어떤 물체인지 구별하는 Object Detection으로 복잡하게 진화하였다.

**Object Detection** 이란 쉽게 말해 영상 속에서 Object의 위치를 찾아내고(**Localization**) 각각이 어떤 종류인지 분류해내는(**Classification**) 문제로 정의할 수 있다. 

![fig1_cv_task](https://user-images.githubusercontent.com/2151950/64918417-e405bd80-d7d8-11e9-804a-01497bc2bf6c.png)

영상으로부터 특정 물체가 포함된 영역이 잘 지정된다면 어떤 물체인지 분류하는 Classification은 위에서 보았듯이 큰 문제가 되지 않는다. 즉, Object Detection에서 핵심이 되는 것은 물체가 어디에 있는지에 해당하는 Localization이다. 우리가 쉽게 생각해볼 수 있는 방법은 입력 영상으로부터 일정 크기의 영역들을 크기를 달리해가며 탐색하는 것이다. 이동하면서 일정 크기의 영역들을 탐색하는 방식을 sliding window라고 하며 단순하지만 정확한 방법이다. 하지만 다양한 물체 크기와 다수의 물체를 찾기에 연산 시간이 턱없이 부족하므로 이를 개선하기 위한 효율적인 연구들이 진행되어 오고있다.

<img width="30%" alt="sliding_window" src="https://user-images.githubusercontent.com/2151950/64918875-1fa38600-d7df-11e9-8e07-33c14ffc3221.png" align="center">

앞으로 Object Detection 문제를 해결하기 위해 개발되어 온 딥러닝 기반의 모델들을 앞으로 하나씩 살펴보려고 한다. 친절하게도 [Hoya012](https://github.com/hoya012/deep_learning_object_detection) 님께서 친절하게 시간 순으로 논문으로 발표된 딥러닝 기반의 Object Detection Architecture를 그림으로 정리해 주셨다. 여기서 빨간색으로 처리된 것들은 핵심 Architecture로 꼭 이해하고 가는 것이 좋다고 한다.