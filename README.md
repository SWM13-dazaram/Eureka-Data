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

## AntiPattern
- **ray.get을 for문 안에서 계속 부르지 마라**
  - for문안에서 계속 ray.get을 하면 해당 함수가 결과를 낼 때 까지 다음 번 명령을 실행하지 못한다
    - 즉 병렬적으로 처리하지 못한다
- ray.get을 자주 호출하지 마라
  - 데이터 변경이 일어나지 않고 다른 ray function에 사용해야된다면 굳이 데이터를 꺼냈다가 넣지 마라
    - 데이터의 불필요한 이동 일어난다
- 큰 데이터를 ray function안에서 사용하게 하지 마라
  - ray.put 을 이용하여 미리 Ray object store에 저장하고 ray.get으로 불러서 사용해라
- ray안에 있는 큰 데이터를 한번에 부르지 마라
  - batch로 나눠서 가져와라 안그러면 OOM(out of memory)일어난다
- 너무 작은 함수를 병렬,분산처리 하도록 하지 마라
- 글로벌 변수를 ray function에서 수정하지 마라
- ray function 지정은 한번만 해라

### 출처
[Ray 공식문서](https://docs.ray.io/en/latest/ray-core/tasks/patterns/)

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
