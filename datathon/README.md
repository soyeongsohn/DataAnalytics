# Datathon


## Contents
    1. 분석 목표
    2. 어떤 Data 인가?
    3. EDA(Exploration Dataa Analysis)
    4. 결론
    5. 회고
---    
### 분석 목표
 - 세일 기간 및 판매 행사를 위해 소비자가 주로 방문하는 요일과 시간대를 파악하고, 구매한 품목 별 관계를 통해 행사 품목을 지정하고자 한다.

---
### 어떤 Data 인가?
|**Folder**|Column|
|-----|---|
|orders|order_id : 주문 ID<br>user_id : 구매자 ID<br>eval_set :  prior/tain/test로 나뉘며, 마지막 주문은 train/test로 나뉜다.<br>order_number : 주문 수<br>order_dow : 주문 요일<br>order_hour_of_day : 주문 시간<br>days_since_prior_order : 재주문 시간<br>(하나의 행은 하나의 주문)|
|order_products_train<br>order_products_prior|order_id : 주문 ID<br>product_id : 상품 ID<br>add_to_cart_order : 해당 상품이 몇 번쨰 순서에 주문이 되었는가?<br>reordered : 재주문 여부|
|products|product_id : 상품 ID<br>product_name : 상품명<br>aisle_id : 진열대 ID<br>department_id : 상품군 ID|
|aisles|aisle_id : 진열대 ID<br>aisle : 진열대명|
|departments|department_id : 상품군 ID<br>department : 상품군명|

---
### EDA
우선 orders data를 먼저 살펴보자

![image](https://user-images.githubusercontent.com/87685922/228862425-588bf24b-318f-459a-9f7d-c3f08c3ad1c8.png)

해당 graph는 eval_set column을 종류 별로 분류한 것이다.

총 data 중 prior이 압도적으로 많음을 보여주고 있다.

train 데이터를 추가해도 결과에 미치는 영향이 미미할 것 같아서 prior만 가지고 EDA를 진행하였다.

order_product_prior와 다른 csv 파일들을 merge하여 하나의 데이터셋으로 만들어준 후 다음을 분석하였다.

1. 어느 제품군에서 많이 팔렸는가?
    
    ![image](https://user-images.githubusercontent.com/87685922/228862634-c700a23a-6db9-4832-9eb4-ffd15b95bcba.png)
    
    농수산물을 의미하는 produce가 제일 많이 팔렸고, 다음으로 달걀/유제품이 많이 팔렸다.
    
2. 어느 진열대에서 많이 팔렸는가?
    
    ![image](https://user-images.githubusercontent.com/87685922/228862753-3f07fd48-aa5f-4684-bd5c-002f6a6e0c7a.png)
    
    신선 과일, 신선 채소와 같은 농산물이 가장 많이 팔렸고, 요거트, 치즈, 우유와 같은 유제품류도 다음으로 많이 팔린 것을 확인할 수 있다.
    
3. 어떤 제품이 많이 팔렸는가?
    
    ![image](https://user-images.githubusercontent.com/87685922/228862824-07a907f6-d352-4866-a2aa-2712d0e87244.png)
    
    바나나가 압도적으로 많이 팔린 것을 확인할 수 있다. 유기농 제품들도 눈에 보인다.
    
4. 제품군 별 진열대 정보 시각화
    
    ![image](https://user-images.githubusercontent.com/87685922/228863258-275e10c8-e5c4-4a25-b971-7d4dbd557409.png)

이번에는 행사 시간을 정하기 위해 시간에 따른 주문량을 알아보자.
1. 요일에 따른 주문량
    
    ![image](https://user-images.githubusercontent.com/87685922/228863740-a7c11fcd-3f66-43b3-9ca7-4ebf7943d953.png)
    
    일요일, 월요일이 주문량이 많음을 알 수 있다.
    
2. 시간에 따른 주문량
    
    ![image](https://user-images.githubusercontent.com/87685922/228863919-a4f2cdda-e596-4bad-8cd8-9df7d3d17f3a.png)
    
    주 활동시간대인 10시 ~ 16시에 주문량이 제일 많다. 심야시간대에는 주문량이 매우 저조한 게 보인다.
    
3. 요일과 시간에 따른 주문량
    
    ![image](https://user-images.githubusercontent.com/87685922/228864009-d994ba72-76c3-4333-9b19-b260874fa391.png)
    
    아침~오후에 주문이 주로 많고, 일요일, 월요일에 주문량이 더 많다. 특히 월요일 오전 (10시) , 일요일 오후(14시)에 주문이 많다.
    
이제 소비성향에 따른 소비자를 Clustering을 통해 알아보자.
행사 품목을 지정하기에 앞서 target을 구분하기 위해 Clustering을 통해 4개의 집단으로 구분하였다.

![image](https://user-images.githubusercontent.com/87685922/228864350-8634bab9-8ac2-4d8c-82f3-8f88948e3d56.png)

색상 별로 0~3까지의 집단으로 구분할 수 있었다.

각 그룹의 차이는
0번째 그룹 - 'frozen meals', 'soft drinks'를 구매했다는 점이 다른 그룹과 다르다.
1번째 그룹 - 'candy chocolate', 'cream', 'fresh dips tapenades', 'tea'를 구매했다는 점이 다른 그룹과 다르다.
2번째 그룹 - 'baking ingredients', 'canned jarred vegetables', 'canned meals beans' ,'fresh herbs', 'oils vinegars', 'soup broth bouillon'를 구매했다는 점이 다른 그룹과 다르다.
3번째 그룹 - 'baby food formula', 'frozen breakfast'를 구매했다는 점이 다른 그룹과 다르다.

마지막으로 위의 각 그룹에 따른 assotiation을 분석을 해보자.
해당 그룹별 품목 간의 관계를 FPgrowth를 통해 알아보았다.

먼저 g0 그룹의 연관분석이다.
![image](https://user-images.githubusercontent.com/87685922/228865457-0b3628b9-1835-43db-bfd3-fab71a7356b6.png)
상위 5개에 대한 결과이며 이에 대해 그래프로 살펴보자.
![image](https://user-images.githubusercontent.com/87685922/228865593-72d4f89d-47aa-41bf-893b-eb6a17e7edf4.png)
다음과 같은 품목들이 서로 연관이 있음을 알 수 있다. 이 때, 화살표의 출발이 조건이며 도착이 결과이다. Ex) A라는 것을 사면 B를 산다.

차례대로 g1부터 g3까지 살펴보겠다.
g1의 결과이다.
![image](https://user-images.githubusercontent.com/87685922/228865926-bec63bc2-2d8d-49db-b0cc-944cba332b7f.png)
![image](https://user-images.githubusercontent.com/87685922/228865973-ce99300c-0579-49c3-802e-dc53d4507052.png)

g2의 결과이다.
![image](https://user-images.githubusercontent.com/87685922/228866041-e91a4b9d-c0c3-4ac3-81bb-0d9736fcbedb.png)
![image](https://user-images.githubusercontent.com/87685922/228866108-4308ecf1-14fd-42f7-9a77-8d3882c29579.png)

마지막으로 g3의 결과이다.
![image](https://user-images.githubusercontent.com/87685922/228866184-37f19142-dc6a-49fa-b87a-d1f26d06edfa.png)
![image](https://user-images.githubusercontent.com/87685922/228866216-5233bda5-a64c-46d1-9efa-0ded9d17b142.png)


---
### 결론

우리가 이 EDA를 통해 얻고자 한 것은 매장 행사를 위해 행사 시기와, 행사 품목을 지정하기 위한 것이다.
1. 행사기간은 평소 사람들이 잘 이용하지 않는 목요일 08시~20시로 지정하였다.
   이유는 사람들은 평소 일요일과 월요일은 이용고객이 많기 때문에 그 외 다른 시간에도 이용 고객을 늘리면서, 판매량을 증가 시키기 위함이다.
2. 행사 품목은 마지막 EDA 그래프인 그룹별 품목 관계도에 있는 묶음 결과 대로 묶음 판매를 하는 것이다.
   여러 고객들이 이용할 수 있도록 Clustering을 통해 그룹 별로 주로 구매하는 품목을 찾았고, 동시에 연관분석을 통해 같이 구매하는 품목 또한 찾았다.
   따라서 고객들이 해당 상품을 같이 구매할 수 있도록 묶음 판매를 하는 것이다.
