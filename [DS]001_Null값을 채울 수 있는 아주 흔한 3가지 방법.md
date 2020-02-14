

Null값을 채울 수 있는 방법은 아래의 3가지가 가장 기본적인 듯 하다.

- **산술평균**(Average)
- **중간값**(Median)
- **최빈값**(Mode)

-----

먼저 Null이 무엇인지 간단하게 살펴보자. 한국말인지 한자어인지 결측치라고 이름을 지었드라. 대략 100번 정도 들으니까 겨우 기억하게 되었다. 



## Null(결측치, Missing Value)

> **Null**은 데이터베이스 내의 **데이터 값이 존재하지 않는다**는 것을 지시하는데 사용되는 특별한 표시어

Python에서는 어떻게 나타나는지 알아보자.

아래의 코드는 Null값이 들어있는 데이터프레임을 마음대로 만들어보았다.

```python
#행
rows = ['영준', '상국', '철희', '예랑']
#열과 값
data = {'국어':[54, 42, np.nan,23],
       '수학':[23,54,32,np.nan],
       '과학':[np.nan,43,65,65],
       '영어':[34,np.nan,56,24]}

df = pd.DataFrame(data, index = rows)
```

위의 코드를 실행하면 아래와 같이 만들어진다.

<img src="/Users/youngjunyoon/Desktop/Github/Data_Analysis with Python/img/[DS]001.png" style="zoom:50%;" />

중간중간 **NaN**이라는 글자가 보일 것이다. 저것이 **결측치**이다.



## 결측치를 채울까? or 제거할까?

> 개인적인 생각으로는 데이터 Series 간의 **상관관계**를 통해서 결측치를 채울지 말지를 정하는게 좋을 듯하다.



## If 결측치를 채운다면,

위에서 언급한 3가지 방법이 있다.

> - **산술평균**(Average)
> - **중간값**(Median)
> - **최빈값**(Mode)



## 1. 산술평균(Average)

**정의** : 나열된 모든 수를 더하고 나열된 수의 개수만큼 나눈 값

#### 코드로 구현해보기1_산술평균

```python
#배열 형태의 데이터 x의 평균값을 찾는 함수 만들기
def mean(x):
		return sum(x)/len(x)  

# return sum(x)/len(x) 
'''
	배열 데이터 x에 있는 모든 수를 더하기 위해 sum(x)를 사용하고
	데이터의 개수만큼 나누어주어야 하기 때문에 len(x)로 개수를 파악하여 나누어준다.
'''
```

배열 데이터 x에 있는 모든 수를 더하기 위해 `sum(x)`를 사용하고 데이터의 개수만큼 나누어주어야 하기 때문에 `len(x)`로 개수를 파악하여 나누어준다.



#### 코드로 구현해보기2_산술평균

```python
#Numpy의 .mean()메소드를 활용
np.mean(x)

'''
 - numpy의 mean 메소드를 사용하면 더욱 간편
 (반드시 np.array(x)를 사용하여 데이터를 바꾸어주어야 연산이 가능)
'''
```

numpy의 mean 메소드를 사용하면 더욱 간편
(반드시 `np.array(x)`를 사용하여 데이터를 바꾸어주어야 연산이 가능)



#### 코드로 구현해보기3_산술평균

```python
#데이터프레임의 Series의 평균값 구하기
df['국어'].mean()
df.loc['영준'].mean()
'''
데이터프레임에서 
한 column만 지정해서 보려면 df['column명']
한 row만 지정해서 보려면 df.loc['row명']
'''
```

데이터프레임에서 한 column만 지정해서 보려면 `df['column명']*` 한 row만 지정해서 보려면 `df.loc['row명']`



## 2. 중앙값(Median)

**정의** : 전체 자료를 크기별로 정렬했을 때 가장 중앙에 있는 값



> [중앙값을 찾는 방법]
>
> - 데이터를 작은 수부터 큰 수 순서대로 정렬.
>
> - 데이터의 개수가 홀수라면 중앙값은 정렬된 결과의 가운데 수이다.  
>
>   > **(N+1)/2번째 데이터값** 
>
> - 데이터의 개수가 짝수라면 중앙값은 가운데 두 수의 평균이다.
>
>   > **N/2번째와 (N+1)/2번째 데이터의 평균값**



예를 들어 9명의 사람의 최고혈압을 측정한 후 낮은 값부터 높은 값까지 순서대로 정렬했을 때 5번째인 사람의 최고혈압의 데이터의 중앙값이 된다. 만약 사람수가 10명이라면 5번째와 6번째 사람의 최고혈압의 평균값을 사용한다.



#### 코드로 구현해보기1_중앙값

```python
#배열 형태의 데이터 x의 중앙값을 찾는 함수 만들기
def median(x):
  sorted_x = sorted(x)
  n = len(x) 
  midpoint = n//2
  
  if n%2 == 1:
    return sorted_x[midpoint]
  
  else:
    mid1 = midpoint - 1
    mid2 = midpoint
    return (sorted_x[mid1]+sorted_x[mid2])/2
```



#### 코드로 구현해보기2_중앙값

```python
#Numpy의 median()메소드 활용
np.median(x)
```



## 3. 최빈값(Mode)

> 데이터 집합에서 가장 많이 등장한 데이터입니다. 최빈값은 데이터 집합에 같은 값이 많이 반복될 때 유용하다. 
>
> 최빈값이 없거나, 하나거나, 여러 개의 최빈값이 있을 수 있다.

```python
#배열 형태의 데이터 x의 최빈값을 찾는 함수 만들기
from collections import Counter
def mode(x):
    counts = Counter(x)
    max_count = max(counts.values())
    return [x_i for x_i, count in counts.items() if count == max_count]
```











