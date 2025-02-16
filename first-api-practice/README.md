
<br>

> **FastAPI 실습**
>
> 🔗 FastAPI의 [공식문서](https://fastapi.tiangolo.com/ko/)를 참고하여 공부했습니다.

<br>
<br>

# 1. FastAPI란?

현대적이고, 고성능이며, 파이썬 표준 타입 힌트에 기초한 Pythond의 API를 빌드하기 위한 웹 프레임워크이다.

<br>

> **API**
>
> Application Programming Interface
> 소프트웨어 애플리케이션 간에 데이터를 주고받을 수 있도록 하는 규칙이나 프로토콜 
>
> API의 주요 기능
> - 컴퓨터나 컴퓨터 프로그램 사이의 연결 
> - 소프트웨어 어플리케이션 간에 데이터, 기능 교환
> - 소프트웨어 프로그램이 다른 소프트웨어 프로그램으로 데이터를 전송
> - 웹 개발에서는 주로 HTTP 요청을 통해 데이터를 주고받음 

<br>
<br>

## 1.1 FastAPI의 주요 특징

<br>


- **빠른 속도**
  - NodeJS 및 Go와 대등할 정도로 매우 높은 성능
  - 사용 가능한 가장 빠른 파이썬 프레임워크 중 하나
- **빠른 코드 작성**
- **적은 버그**
  - 개발자에 의한 에러가 약 40% 감소
- **직관적**
  - 적은 디버깅 시간
  - 모든 곳에서 자동완성
- **쉬움**
  - 쉽게 사용하고 배우도록 설계
- **짧은 코드**

  - 코드 중복 최소화

<br>
<br>
<br>

# 2. fastapi와 uvicorn 설치

<br>

```bash
$ pip install fastapi
```

```bash
$ pip install "uvicorn[standard]"
```

<br>

서버를 구축하기 앞서 fastapi와 uvicorn 모듈을 설치해준다.


<br>
<br>
<br>

# 3. `main.py`

## 3.1 서버 만들기
<br>

```python
from typing import Union

from fastapi import FastAPI

app = FastAPI()


@app.get("/")
def read_root():
    return {"Hello": "World"}


@app.get("/items/{item_id}")
def read_item(item_id: int, q: Union[str, None] = None):
    return {"item_id": item_id, "q": q}
```

<br>
<br>
<br>

## 3.2 서버 실행하기 

```bash
$ uvicorn main:app --reload
```

위의 명령어로 서버를 실행하면 다음과 같이 된다.

참고로 uvicorn은 애플리케이션을 로드하고 제공하는 서버이다.

<br>
<br>


![](https://velog.velcdn.com/images/xxubin/post/30e0c9e8-414f-4407-bd9f-0f7cd515b275/image.png)


<br>
<br>
<br>

## 3.3 서버 확인하기

<br>

```python
# main.py
@app.get("/")
def read_root():
    return {"Hello": "World"}
```


<br>

<p align="center"><img src="https://velog.velcdn.com/images/xxubin/post/29c9a7e8-a1ca-4b5f-ab25-aebc7f793dd7/image.png" width="500"></p>

브라우저로 `http://127.0.0.1:8000/`에 접속하면 위의 JSON 응답을 볼 수 있다.

그러면 이번에는 `http://127.0.0.1:8000/items/5?q=notebook`로 접속해보자.


<br>
<br>
<br>


```python
@app.get("/items/{item_id}")
def read_item(item_id: int, q: Union[str, None] = None):
    return {"item_id": item_id, "q": q}
```

<br>


<p align="center"><img src="https://velog.velcdn.com/images/xxubin/post/ba7e06cd-e196-4204-b74b-71fdb81ab95e/image.png" width="500"></p>



<br>
<br>


#### 분석

```
http://127.0.0.1:8000/

http://127.0.0.1:8000/items/5?q=notebook
```

<br>

`127.0.0.1:8000`
  - `127.0.0.1` : 기본 로컬 호스트를 나타내는 특별한 IP 주소 

  - `:8000` : fastAPI의 기본 포트번호 

<br>

`/`, `/items/5?q=notebook`
  - `/`, `/items/{item_id}`에서 HTTP 요청을 받는다.

  - GET 메소드인 `@app.get()`을 사용한다.

<br>


`def read_item(item_id: int, q: Union[str, None] = None)`
   - `item_id` : 경로 매개변수가 int형이어야 한다.
     - 현재 '5'를 `item_id`의 매개변수로 받고 있다.
   - `q` : 경로 매개변수가 선택적인 str형이어야 한다. 
   
     - 현재 'notebook'을 `q`의 매개변수로 받고 있다. 

<br>
<br>
<br>
<br>

### Union[str, None] = None 

<br>

```python
def read_item(item_id: int, q: Union[str, None] = None)
```

- `Union[str, None]`은 선택적인 문자열인 `(str | None)`을 의미한다. q에 아무 값도 안 주면 `None`이 기본 값이 되고, 값을 주면 `str`으로 들어온다. 

- `Union[str, None]`이 매개변수로 str형이나 None이 들어올 수 있다는 것이고, `= None`이 기본값을 None으로 설정하는 것이다. 

<br>
<br>

#### 예제

```bash
# 문자열 전달 
http://127.0.0.1:8000/items/1?q=hello
# 결과: {"item_id": 1, "q": "hello"}

# q 생략
http://127.0.0.1:8000/items/1
# 결과: {"item_id": 1, "q": null}
```

<p align="center"><img src="https://velog.velcdn.com/images/xxubin/post/fd5016f6-78c7-4b72-90bb-4559d2a1b3a7/image.png" width="500"></p>


<p align="center"><img src="https://velog.velcdn.com/images/xxubin/post/7b69ac22-6da7-4028-ba47-f458b52c6b11/image.png" width="500"></p>




<br>
<br>
<br>
<br>

# 4. API 문서

<br>

#### 대화형 API 문서

```bash
http://127.0.0.1:8000/docs
```

<br>

#### 대안 API 문서


```bash
http://127.0.0.1:8000/redoc
```

<br>
<br>
<br>
<br>


# 5. PUT 요청 추가

Pydantic을 이용해 파이썬 표준 타입으로 Request Body를 선언할 수 있다.

<br>

```python
from typing import Union

from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

# 추가 
class Item(BaseModel): 
    name: str
    price: float
    is_offer: Union[bool, None] = None
    

@app.get("/")
def read_root():
    return {"Hello": "World"}


@app.get("/items/{item_id}")
def read_item(item_id: int, q: Union[str, None] = None):
    return {"item_id": item_id, "q": q}

# 추가 
@app.put("/items/{item_id}")
def update_item(item_id: int, item: Item):
    return {"item_name": item.name, "item_id": item_id}
```

<p align="center"><img src="https://velog.velcdn.com/images/xxubin/post/f959eaad-0d76-41fc-bdf8-8246fcc561af/image.png" width="800"></p>

