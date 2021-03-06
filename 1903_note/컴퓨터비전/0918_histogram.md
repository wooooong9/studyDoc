## 컴퓨터비전

0918

### 히스토그램

영상이 들어왔을 때 밝기값이 어떻게 분포되어 있는지 표로 나타낸 것.

![스크린샷 2019-09-18 오후 1.45.42](/Users/gilwoongkang/1903_note/컴퓨터비전/image/스크린샷 2019-09-18 오후 1.45.42.png)



영상에서 비춰지는 밝기의 정도를 표기한다. 

##### 히스토그램 평활화(Histogram Equalization)

히스토그램을 평평하게 만들어 주는 연산이다. 명암의 동적 범위를 확장하여 영상의 품질을 높힌다. 
$$
l_{out}=T(l_{in})=round(c(l_{in}) \times (L-1)) \\ c(l_{in}=\sum^{l_{in}}_{t=0}h(l))
$$
만일 히스토그램 스트레칭 알고리즘을 만드려면, min 값과 max값을 찾아서 계산해야한다. 
$$
l_{s} = \frac{255} {(max - min)} = l
$$
히스토그램의 누적도수 분표포의 기울기를 확인해 기울기를 기준으로 weigh을 넣어 늘려주게 되면 평활화가 가능하다. 

더 큰 기울기 부분에 대해서는 많이 늘려주고 적은 기울기 부분에 대해서는 조금만 늘리는 방식으로 평활화를 한다.

![스크린샷 2019-09-18 오후 2.06.19](/Users/gilwoongkang/1903_note/컴퓨터비전/image/스크린샷 2019-09-18 오후 2.06.19.png)

##### 히스토그램 역투영

히스토그램을 매핑 함수로 사용하여, 화소값을 신뢰도 값으로 변환한다.

![스크린샷 2019-09-18 오후 2.14.33](/Users/gilwoongkang/1903_note/컴퓨터비전/image/스크린샷 2019-09-18 오후 2.14.33.png)

> *색을 나타내는 3채널에서 두채널만 값을 표기하는 이유는 두 값만 알면 나머지 한값을 계산해서 알 수 있기 때문에 표기하지 않는다.(원래 HSV인데 HS만 표기.)*



- 2차원 히스토그램의 계산

입력 : H와 S채널 영상 
$$
f_H(i,j),f_s(j,i),0 \leq j \leq M-1,0 \leq i \leq N-1
$$
출력 : 히스토그램 과 정규 히스토그램 
$$
h(j,i)\\\hat h(j,i),0\leq j,i\leq q-1
$$
히스토그램 역투영은 배경을 조정할 수 있는 상황에 적합하지만, 비슷한 색 분포를 갖는 다른 물체를 구별하지 못한다. 즉 검출 대상이 여러 색 분포를 갖는 경우에 오류 가능성이 높다.

##### 이진화 

어떠한 T값에 대해서 이 값보다 높다면 1로 더 작다면 0으로 표기하는 방법을 이진화라고 한다. 

그런데 이 이진화의 한가지 문제점은 기준점(slash hold)을 어떻게 잡을 것이나 하는 문제가 있다. 이를 위해 오츄 알고리즘을 이용한다.

##### 오츄 알고리즘

좌 우측으로 나누었을때 좌 우측의 분산값의 차이가 최소가 되도록 만드는 부분을 중간값으로 정하는 알고리즘.

![스크린샷 2019-09-18 오후 2.26.14](/Users/gilwoongkang/1903_note/컴퓨터비전/image/스크린샷 2019-09-18 오후 2.26.14.png)

하지만 일일히 계산하게 되면 비효율 적이므로 히스토그램을 이용해서 좀 더 빠르게 구한다. 

t-1 번째의 계산 결과를 t번째에 활용하여 빠르게 계산한다.

![스크린샷 2019-09-18 오후 2.34.44](/Users/gilwoongkang/1903_note/컴퓨터비전/image/스크린샷 2019-09-18 오후 2.34.44.png)

##### 연결 요소 

![스크린샷 2019-09-18 오후 2.42.08](/Users/gilwoongkang/1903_note/컴퓨터비전/image/스크린샷 2019-09-18 오후 2.42.08.png)

![스크린샷 2019-09-18 오후 2.42.15](/Users/gilwoongkang/1903_note/컴퓨터비전/image/스크린샷 2019-09-18 오후 2.42.15.png)

어떠한 연결성으로 보느냐에 따라 원하는 것을 볼수 있을지 없을지가 나뉘는데, 4연결성이 더 빠르기 때문에 4연결성만으로 볼수 있다면 그렇게 하는것이 좋다.