# 멀티 프로세싱
만개의 레시피 크롤링 하려고 하는데 1페이지당 40개 레시피 존재하는데 9초정도 걸렸음
18만개 존재하여 다 가져오려면 11시간 정도 걸림 
그래서 멀티 프로세싱 찾아봄

파이썬의 mulitprocessing보다 ray 라이브러리 성능 좋음

m1에서 ray 사용시 dashboard 어쩌고 하면서 이슈있음
```
pip uninstall grpcio; conda install grpcio
```
이거 사용해서 오류 없애줌,,

# 데이터 크롤링
# 우리의 식탁

## 순서

1. 레시피 리스트 API 호출 후 저장
2. token 이용하여 레시피 상세정보 API 호출 후 저장

## 레시피 리스트 API

https://wtable.net/api_v2/recipe/list?app_version=1&platform=web&uuid=26a7a986-66be-4399-b09c-b2d9e27c7a56&order=publish_desc&offset={offset}&limit={limit}

offset → 0부터 시작

limit → 10000개 이상도 가능 지금 1746개 존재

uuid → 정확히 무슨 역활인지 모르겠음

크롬 시크릿 모드로 하면 b4220af9-70f8-4d67-96cf-1526cbba4b8c 나옴

그래서 일단은 빼고 불러옴 → 그래도 동작함

## 레시피 상세정보 API

[https://wtable.co.kr/_next/data/QyVQmbVsmSdCqTclsMsAZ/recipes](https://wtable.co.kr/_next/data/QyVQmbVsmSdCqTclsMsAZ/recipes)/{token}.json?location=recipe_home&token={token}

token → 레시피 리스트 API로 가져옴

QyVQ → 이거 뭔지 모르겠음 시크릿 모드로 들어가도 똑같이 나옴