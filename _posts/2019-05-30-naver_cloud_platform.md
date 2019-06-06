---
layout: post
title: "Naver Cloud Platform AI 서비스 이용해보기 - Object Detection"
author: JH
date:   2019-05-30
tag: [객체검출, 영상처리, API, Naver AI service]
description: "Naver에서 Cloud Platform을 제공하며 그 중 API를 이용한 AI service가 있다. 네이버의 풍부한(?) 데이터를 기반으로 제공되는 객체 검출 서비스를 한 번 적용시켜보자."
categories: [Image Processing, Naver Cloud Platform]
---

## Naver Cloud Platform AI service API 호출을 위한 함수
- Argument: 검출하고 싶은 영상의 경로 `image_path` 를 입력해준다
- Return: Naver API 호출을 통해 받은 Object detection 결과 `response` 를 json 파싱을 하여 가져오도록 한다


```python
import os
import sys
import requests
import time
import json

def api_call(image_path):

    json_file = open('key.json')
    json_data = json.load(json_file)

    client_id = json_data['client_id']
    client_secret = json_data['client_secret']
    url = "https://naveropenapi.apigw.ntruss.com/vision-obj/v1/detect"

    files = {'image': open(image_path, 'rb')}
    headers = {'X-NCP-APIGW-API-KEY-ID': client_id, 'X-NCP-APIGW-API-KEY': client_secret }

    response = requests.post(url,  files=files, headers=headers)
    rescode = response.status_code

    return json.loads(response.text)
```
---


## 검출 결과를 표시(bounding box)하기 위한 후처리 함수
- Argument: 검출에 사용된 영상의 경로 `image_path` 와 API 호출을 통해 반환 받은 결과 `detections` 을 입력해준다
- Reeturn: `detections` 으로부터 검출된 정보를 OpenCV library를 이용하여 Visualization을 해준다


```python
import cv2
from matplotlib import pyplot as plt
import numpy as np

import colors

def postProcessing(image_path,detections):

    image = cv2.imread(image_path)
    height, width, _ = image.shape
    for label,name, score, bbox in zip(detections['detection_classes'],detections['detection_names'],detections['detection_scores'],detections['detection_boxes']):

        b = np.array(bbox)
        b[0],b[1] = b[1],b[0]
        b[2],b[3] = b[3],b[2]

        b[0] *= width
        b[2] *= width
        b[1] *= height
        b[3] *= height

        b= b.astype(int)

        color = colors.label_color(int(label))
        cv2.rectangle(image, (b[0], b[1]), (b[2], b[3]), color, 2, cv2.LINE_AA)

        caption="{} {:.3f}".format(name, score)
        #draw_caption(draw, b, caption)
        cv2.putText(image, caption, (b[0], b[1] - 10), cv2.FONT_HERSHEY_PLAIN, 1, (0, 0, 0), 2)
        cv2.putText(image, caption, (b[0], b[1] - 10), cv2.FONT_HERSHEY_PLAIN, 1, (255, 255, 255), 1)

    return image
```
---

## main part


```Python
image_path = 'sample.jpg'

detections = api_call(image_path)
detections = detections['predictions'][0]
image = postProcessing(image_path,detections)


image = cv2.cvtColor(image,cv2.COLOR_BGR2RGB)
plt.figure(figsize=(20, 20))
plt.axis('off')
plt.imshow(image)
plt.show()
```
---

## 결과 영상
![결과 영상](https://user-images.githubusercontent.com/2151950/58641755-c0585d80-8336-11e9-835c-0e39ad628c99.jpg)
