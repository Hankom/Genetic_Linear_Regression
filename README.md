# 최적화 알고리즘을 이용한 회귀식 추정 보고서

> **컴퓨터 알고리즘, 김동훈 교수님**
>
> > hankom(한경환)

## 환경 설정

- OS: Windows 11 Home
- GC: GTX 1080
- RAM: 16GB
- CPU: i5-8400
- Lang: Python 3.7

## 선형 회귀 분석

선형 회귀 분석은 독립변수와 종속변수 사이에 직선적인 형태의 관계가 있다고 가정을 한다.

직선적인 형태란 독립변수가 일정하게 증가하면, 종속변수도 그에 비례해서 증가하거나 또는 감소하는 형태를 말한다.

예를 들어 밥을 한 그릇 더 먹으면 체중이 정확히 100g 증가한다면 직선적인 형태라고 할 수 있다. 밥을 한 그릇, 두 그릇, 세 그릇, .. 먹으면 체중이 100g, 200g, 300g으로 증가하기 때문에 이것을 그림으로 그려보면 직선이 그려진다.

## 회귀계수

선형 모형에서 x와 y의 관계를 수식으로 나타내면 아래와 같다.

y = wx + b

위의 식에서 w를 회귀계수(cofficient)라고 한다. 독립변수 x가 1 증가할 때마다 종속변수 y는 w만큼 증가한다.

밥을 1 그릇 먹을 때마다 체중이 100g씩 증가하면 밥 그릇 수에 100을 곱하면 체중의 변화량이 된다. 따라서 이때의 회귀계수는 100이라고 할 수 있다.

그림으로 그리면 회귀계수는 직선의 기울기(slope)가 된다.

위의 식에서 독립변수 x=0이면 y=b가 된다. 이를 y-절편, 또는 간단히 절편(intercept)이라고 한다.

- 절편(intercept): 독립변수가 모두 0일때 종속변수 y의 값.

---

## 회귀 모델

미국 1 달러 기준, 영국 파운드의 환율과 일본 엔의 환율을 비교하는 모델을 설정하였다. 최적 알고리즘으로는 유전 알고리즘을 택하였다.

데이터는 USDJPY.csv, USDGBP.csv
81/01/21 ~ 17/05/26 사이의 환율을 기록해놓은 것이다.

- 독립변수: 일본 엔 환율(JPY)
- 종속변수: 영국 파운드 환율(GBP)

### 동작 방식

최적화 알고리즘을 도입하기 전, 선형회귀모델로서 회귀 계수(모수)와 절편을 구해본다.

![1](https://raw.githubusercontent.com/Hankom/Genetic_Linear_Regression/main/img/1.png)

![2](https://raw.githubusercontent.com/Hankom/Genetic_Linear_Regression/main/img/2.png)

각각의 환율 데이터(X, Y)들을 Scikit-Learn을 이용하여 0과 1의 수로 정규화 시켜준다.

![6](https://raw.githubusercontent.com/Hankom/Genetic_Linear_Regression/main/img/6.png)

Scikit-learn의 선형 모델 함수를 이용하여 선형 회귀 모델을 설정하고 회귀 계수와 절편을 구한다.

### 최적화 알고리즘 (유전 알고리즘)

![44](https://raw.githubusercontent.com/Hankom/Genetic_Linear_Regression/main/img/44.png)

- fitness(적합): 적합 함수로 최소자승법을 이용한다.
- genPopulation(크기): 무작위의 크기의 인구를 발생시킨다. 크기는 400으로 지정한다.
- crossover: 최적으로 선정된 값끼리 회귀계수는 유지하며, 절편은 최적으로 선정된 값끼리 반영한다.
- selectBest(최적): 최적으로 선정된 20개의 값을 리턴한다.
- mutation(돌연변이): 과적합(over-fitting)을 방지하기 위해 돌연변이를 발생시킨다. 돌연변이의 최대 크기는 0.15 이하로 하고, 이하의 난수들을 발생시킨다.

![5](https://raw.githubusercontent.com/Hankom/Genetic_Linear_Regression/main/img/5.png)

에포크 횟수는 600으로 지정하고, newPopulation을 정의하여 최적의 값들을 저장한다.

## 성능 측정

![3](https://raw.githubusercontent.com/Hankom/Genetic_Linear_Regression/main/img/33.png)

선형회귀모델

- 회귀 계수(모수): 1.7007
- 절편: -0.6562
- R의 제곱: 0.9387

R 제곱은 모형 적합도를 뜻한다. JPY의 분산을 GBP가 약 94%정도를 설명함을 뜻한다.

![7](https://raw.githubusercontent.com/Hankom/Genetic_Linear_Regression/main/img/7.png)

유전 알고리즘 사용

- 회귀 계수(모수): 1.6857
- 절편: -0.6422
- R의 제곱: 0.9386

모형 적합도가 약 94% 정도로 의미 있는 모수 값임을 알 수 있다. 대신 계수와 절편 값에서 선형회귀모델에서 구한 값과 조금의 차이가 나는데 유전 알고리즘의 crossover 정의에서 절편끼리 값이 반영되는 과정에서 차이가 발생이 된 것으로 유추한다.

![ani](https://raw.githubusercontent.com/Hankom/Genetic_Linear_Regression/main/img/ga_animation.gif)

# 결론

각각 하나의 독립변수와 종속변수를 갖는 선형회귀모델을 이용하여 영국과 일본의 환율에 관한 유추 모델을 만들어보았다. Scikit-learn을 이용해 이미 선형회귀모델 에서 직접적으로 에러가 최소화된 모수 값을 구할 수 있었지만 유전 알고리즘을 이용하여 구한 모수 값과 비교하여 알고리즘의 성능을 확인해보는 기회가 되었다.
