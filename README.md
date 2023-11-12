# Shopping_mall
Hybrid Cloud Engineer Training Course_Web_Project2

## Shopping Mall 사이트
http://cafekiosk.pythonanywhere.com/

# 프로젝트 소개
* 처음 회원가입한 유저는 관리자 권한을 갖습니다. (1번 유저)
* 2번째부터 회원가입한 유저는 일반 사용자 권한을 갖습니다. (나머지 유저)
* 관리자는 상품을 추가하고 수정, 삭제할 수 있습니다.
* 일반 사용자들은 장바구니에 물건을 담고 주문을 할 수 있습니다.
* 관리자는 물건을 주문할 수 없지만 주문을 처리하거나 취소 또는 삭제할 수 있습니다.

## 웹사이트 소개
https://hel5.tistory.com/56

## Shopping Mall 사이트 화면
![8](https://github.com/JJeong5/Shopping_mall/assets/92209877/fc99d5fa-a676-42f3-9073-d4f2272f50f0)
![7](https://github.com/JJeong5/Shopping_mall/assets/92209877/596c7b8b-616f-41c4-9e49-83f8b916e8fe)
![3](https://github.com/JJeong5/Shopping_mall/assets/92209877/5bd3f4a3-6cc8-4473-ae8f-c8c91d2cf891)
![11](https://github.com/JJeong5/Shopping_mall/assets/92209877/333bdf0f-64d2-49c3-8af4-da2564cf2389)
![12](https://github.com/JJeong5/Shopping_mall/assets/92209877/db842a57-934f-4f05-80ba-bae7d54d5aa8)
![13](https://github.com/JJeong5/Shopping_mall/assets/92209877/59abd3d2-90da-45a4-8593-414ae640bdb3)
![14](https://github.com/JJeong5/Shopping_mall/assets/92209877/d6e1d072-7f91-49b9-9123-c7648cde4b93)
![15](https://github.com/JJeong5/Shopping_mall/assets/92209877/152d6de8-a0d0-4806-bd10-ac767339b0c5)


## 설치 명령어 (Environment Setup)
python -m venv venv

.\venv\Scripts\activate

pip install flask

python -m pip install --upgrade pip

pip install flask-migrate

pip install flask-wtf

set FLASK_APP=shop

set FLASK_DEBUG=true

flask db init

flask db migrate 

flask db upgrade 

flask run

----
# ERD
![데이터베이스 스키마 관계도2](https://github.com/JJungMe/Shopping_mall/assets/92209877/0df70ac9-1e16-46b2-9f5b-0887a4ebe705)
