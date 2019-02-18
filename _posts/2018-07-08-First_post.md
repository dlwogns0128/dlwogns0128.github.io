---
layout: posts
title: Windows 환경에서 jekyll 실행시 CP949 에러 해결 방법 

---

### Windows 환경에서 Jekyll을 이용하여 Github에 블로그 운영을 할 때 

Github로 블로그 운영을 하려고 잘 만들어진 Jekyll 테마를 가져다가 적용시킬 때 아주 골치 아팠던 error가 있었다.

![cp949 error](https://drive.google.com/uc?id=1AxHKSKmPnU0v6OsUUcrhth0fEk9eqxgj)

윈도우 환경에서 Gitbash를 이용해 Ruby 기반의 Jekyll을 사용하려고 하면 발생하는 에러로 기본적으로 Encoding 이 맞지 않아 생기는 문제이다.

### Jekyll Encoding 해결 방법

윈도우 에서 sass 플랫폼 기반의 프레임워크를 실행 시, `Invalid CP949 Character` 에러가 발생한다면, 대부분의 Googling 결과는 _config.yml 파일에서 Encoding 셋팅을 제시한다. 그러나 윈도우에서는 이 방법도 잘 동작하지 않는 것으로 보인다. 왜냐하면 Ruby on Windows 가 기본적으로 CP949 Encoding 으로 동작하기 때문이다. 따라서 Git bash 터미널이 아닌, Ruby on Windows 터미널에서 아래 명령어를 실행하고 Jekyll 을 다시 실행하자

```bash
chcp 65001 # 윈도우 터미널에서 사용할 Encoding(Code Page)변경
$ bundle exec jekyll serve
```




* * *
>[Window 환경에서 jekyll 실행 시 CP949에러](https://jprogram.github.io/articles/2017-12/Windows)
