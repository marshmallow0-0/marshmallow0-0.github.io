---
layout: single
title:  "데이터 분석 프로젝트(steam)"
categories: BootCamp, 스마트팩토리, 코딩온
tag: [Python, 시각화, Pandas]
author_profile: false
# sidebar: 
#     nav: "docs"
search: false

excerpt: "회고록"
---

## 1차 프로젝트

### 프로젝트 설명

부트캠프에서 학습한 데이터 전처리와 시각화했던 경험을 토대로 데이터 분석 프로젝트를 수행하였습니다. 이 과정에서 겪었던 개인적인 성과와 학습 내용을 정리하기 위해 이 회고록을 작성합니다. 

저희 팀은 데이터 시각화라는 기술적인 활용에 초점을 두었습니다. 데이터를 수집하는 과정을 최소화하며 다양한 자료를 가공할 목적으로 전반적인 주제 탐색을 진행하였습니다. 그 결과 모든 조건을 부합한 주제로 저희는 '스트레스'를 메인주제로 선정하였습니다. 주제가 정해짐에 따라 각자 주어진 세부주제에 맞게 데이터를 전처리하고 시각화하기로 결정하였습니다. 그리고 결과들을 추합하는 과정에서 유의미한 부분만 추출하는 것으로 논의하였습니다.

---

### 개인 진행 과정

#### 데이터 전처리

```python

import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from IPython.display import Image
import seaborn as sns

# 데이터 파일 읽기
sad = pd.read_csv("../data/sad.csv", encoding="euc-kr")
stress = pd.read_csv("../data/stress.csv", encoding="euc-kr", header=None)
migration = pd.read_csv("../data/migration.csv", encoding="euc-kr")
blood = pd.read_csv("../data/pressure.csv")
smoke = pd.read_csv("../data/smoke.csv",  encoding="euc-kr")
crimal = pd.read_csv("../data/crimal.csv", encoding="euc-kr")

# 데이터 프레임 정보 확인
print(sad.head())
print(stress.head())
print(migration.head())
print(blood.head())
print(smoke.head())
print(crimal.head())

# 한글 폰트 설정
plt.rc('font', family='Malgun Gothic')

# 시군구별로 인덱스 설정 및 원래 열 제거
sad.set_index('시군구별(1)', inplace=True)
migration.set_index('행정구역(시군구)별', inplace=True)
blood.set_index('시군구별(1)', inplace=True)
smoke.set_index('시군구별(1)', inplace=True)
crimal.set_index('시군구별(1)', inplace=True)

# 첫 번째 열을 인덱스로 설정
stress.set_index(stress.columns[0], inplace=True)
```
![image](/img/python_proj_data.png)

위의 코드와 같이 먼저 전체적인 헤드를 파악하여 각각의 파일에 맞는 인덱스 설정과 헤더를 지정하여 데이터 가공과정을 거쳤습니다. 

---

#### 데이터 내용 파악

데이터의 전체적인 흐름을 확인하기 위해, 지난 10년 동안의 지역별 및 연도별 스트레스 수준을 한눈에 볼 수 있도록 히트맵을 시각화했습니다. 이를 통해 각 지역에서 시간에 따라 스트레스가 어떻게 변화했는지를 쉽게 파악할 수 있었으며, 특히 평균적으로 지난 10년 동안 스트레스가 높은 지역들을 확인할 수 있었습니다.

```python
import seaborn as sns
import pandas as pd
import matplotlib.pyplot as plt

plt.figure(figsize=(10, 8))
sns.heatmap(stress.T, annot=True, cmap='Reds', fmt=".1f")

#'Malgun Gothic' (윈도우 한글 폰트)
plt.rcParams['axes.unicode_minus'] = False

# 각 지역별 스트레스 지수의 평균 계산
stress_mean_by_region = stress.mean(axis=1)

# 스트레스 지수가 높은 순서대로 정렬
stress_mean_sorted = stress_mean_by_region.sort_values(ascending=False)

# 결과 출력
print("지역별 스트레스 지수 평균 (높은 순):")
print(stress_mean_sorted)
```
![image](/img/python_proj_stress.png)

---

#### 범죄율이 스트레스에 미치는 영향 분석

저희 팀이 상관요소로 선정했던 범죄율이 스트레스가 가장 높았던 인천 지역에 반영이 되어있는지 확인해보고자 그래프를 그려보았습니다. 그 결과 인천의 범죄율이 떨어지는 것을 확인하였고 지역별 범죄율 평균을 비교해본 결과 인천이 낮은 순위에 있는 것을 확인하여 간단하게 스트레스와 범죄율이 큰 상관은 없을 것 같다고 예측해볼 수 있었습니다.

![image](/img/python_proj_crimal.png)

---

#### 종속변수(우울, 고혈압, 흡연율) 간 상관관계 히트맵

세부주제들로 우울, 이동, 고혈압, 흡연율 등 다양한 자료들을 수집할 수 있었기에 추가적으로 종속변수들간의 상관을 확인해보고 싶었습니다. 이를 위해 각 변수에 대한 평균을 구한 전체 상관계수를 구하고, 데이터 프레임으로 변형하여 히트맵으로 시각화해보았습니다.  
  
``` python
import matplotlib.pyplot as plt
import seaborn as sns


correlation_matrix = pd.DataFrame({
    '우울감': sad.mean(axis=1),
    '스트레스': stress.mean(axis=1),
    '이동': migration.mean(axis=1),
    '고혈압': blood.mean(axis=1),
    '흡연율': smoke.mean(axis=1),
    '범죄율': crimal.mean(axis=1)
}).corr()

# 상관계수 행렬을 히트맵으로 시각화
plt.figure(figsize=(10, 8))
sns.heatmap(correlation_matrix, annot=True, cmap='coolwarm', vmin=-1, vmax=1)

# 그래프 제목 설정
plt.title('변수 간 상관관계 히트맵')

# 그래프 출력
plt.show()
```

종속결과 상관 계수를 통해 스트레스와 우울, 고혈압에 미약한 상관계수가 존재한다는 것을 확인하게 되었습니다.

- 우울감과 스트레스: 우울감이 높아질수록 스트레스도 높아질 가능성이 높다
- 고혈압과 흡연율: 흡연율이 높을수록 고혈압이 발생할 가능성이 있다
- 범죄율과 고혈압, 흡연율: 범죄율이 증가할수록 고혈압과 흡연율도 증가할 수 있다
- 스트레스와 고혈압: 스트레스가 높을수록 고혈압이 약간 낮아지는 경향이 있다

![image](/img/python_proj_corr.png)

---

#### 소득 구간별 스트레스 비율 파이 차트

``` python
import pandas as pd

# 파일을 열고 데이터를 줄 단위로 읽어오기
with open("datas/stress_income.csv", 'r', encoding='utf-8') as file:
    lines = [line.strip().split(',') for line in file]

# 열의 개수가 맞지 않는 행 제거
valid_lines = [line for line in lines if len(line) == 11]

# 데이터프레임 생성 (컬럼을 지정하지 않음)
stress_income = pd.DataFrame(valid_lines[0:])

# 결과 확인
# 행 0~5까지의 데이터
stress_total = stress_income.iloc[0:5]
stress_total.set_index(0, inplace=True)
stress_total = stress_total.apply(pd.to_numeric)
middle_mean = stress_total.loc[['중하', '중', '중상']].mean().mean()
# stress_total_means = {
#     '하': stress_total.loc['하'].mean(),
#     '중하': stress_total.loc['중하'].mean(),
#     '중': stress_total.loc['중'].mean(),
#     '중상': stress_total.loc['중상'].mean(),
#     '상': stress_total.loc['상'].mean(),
# }
stress_total_means = {
    '하위소득': stress_total.loc['하'].mean(),
    '중위소득': middle_mean,
    '상위소득': stress_total.loc['상'].mean(),
}
# 원형 차트 그리기
explode = [0.05, 0.05, 0.05]
colors = ['#ff9999', '#ffc000', '#8fd9b6']
plt.figure(figsize=(8, 8))
plt.pie(stress_total_means.values(), labels=stress_total_means.keys(), autopct='%1.1f%%', startangle=140,
         counterclock=False, explode=explode, shadow=True, colors=colors)
plt.legend(stress_total_means.keys(), title="소득 수준", loc="center left", bbox_to_anchor=(1, 0, 0.5, 1))

plt.title('소득수준별 스트레스 비율')
plt.show()

```

위의 코드는 CSV 파일에서 데이터를 읽어들여 필요한 부분만 추출하고, 데이터를 통합하는 과정이 필요하였습니다. 원래 5개의 소득 분류(상, 중상, 중, 중하, 하)를 3개의 분류(상, 중, 하)로 범주화하였으며, '중하', '중', '중상'을 하나의 '중위소득'으로 통합하여 평균값을 계산했습니다. 그 후, 각 소득 수준별 스트레스 비율을 시각화한 원형 차트를 그렸습니다.

![image](/img/python_proj_pie.png)

---

#### 스트레스 해소 방법에 대한 워드 클라우드 시각화

``` python
import PyPDF2
from collections import Counter
from konlpy.tag import Okt
from wordcloud import WordCloud
import matplotlib.pyplot as plt

plt.rcParams['font.family'] ='Malgun Gothic'
plt.rcParams['axes.unicode_minus'] =False

# PDF에서 텍스트 추출
def extract_text_from_pdf(pdf_path, start_page=None, end_page=None):
    with open(pdf_path, 'rb') as file:
        reader = PyPDF2.PdfReader(file)
        text = ""
        
        # start_page와 end_page의 값 설정
        if start_page is None:
            start_page = 0  # 시작 페이지가 지정되지 않은 경우 첫 페이지부터 시작
        if end_page is None:
            end_page = len(reader.pages)  # 끝 페이지가 지정되지 않은 경우 마지막 페이지까지
        
        # 지정된 페이지 범위에서 텍스트 추출
        for page_num in range(start_page, end_page):
            text += reader.pages[page_num].extract_text()
    
    return text


# 텍스트 전처리 및 형태소 분석
def analyze_text(text):
    okt = Okt()
    nouns = okt.nouns(text)
    # 한 글자 이상의 단어만 포함하고, "대", "위", "한", "해"를 포함하지 않은 단어만 필터링
    filter_words = ["대", "위", "한", "해", "스"]
    nouns = [noun for noun in nouns if len(noun) > 1 and not any(fw in noun for fw in filter_words)]
    return nouns

# 워드 클라우드 생성 함수
def generate_wordcloud(word_freq):
    wordcloud = WordCloud(
        font_path='C:/Windows/Fonts/malgun.ttf',  # 폰트 경로
        width=800,  # 이미지의 너비
        height=400,  # 이미지의 높이
        max_words=100,  # 최대 단어 수
        max_font_size=100,  # 최대 폰트 크기
        min_font_size=10,  # 최소 폰트 크기
        background_color='white',  # 배경 색상
        colormap='coolwarm',  # 색상 맵 설정
        contour_color='black',  # 외곽선 색상
        contour_width=2  # 외곽선 두께
    ).generate_from_frequencies(word_freq)

    # 그래프 설정 및 시각화
    plt.figure(figsize=(12, 8))  # 그림 크기 설정
    plt.imshow(wordcloud, interpolation='bilinear')
    plt.axis("off")  # 축 제거
    plt.title("스트레스 해소 방법 워드 클라우드", fontsize=24, color='navy', pad=20)  # 제목과 색상, 간격 설정
    plt.show()

# PDF 파일의 40번 페이지부터 41번 페이지까지 텍스트를 추출
pdf_text = extract_text_from_pdf('stress.pdf', start_page=40, end_page=41) 

# 형태소 분석을 통한 키워드 추출
keywords = analyze_text(pdf_text)

# 키워드 빈도 분석
word_freq = Counter(keywords)

# 워드 클라우드 생성 및 출력
generate_wordcloud(word_freq)

# 이미지 파일로 저장 (선택 사항)
# plt.savefig('wordcloud.png')
```

![image](/img/python_proj_wordcloud.png)

마지막으로 결론을 도출하는 과정에서 웹 크롤링 기술을 도입하자는 아이디어가 나왔습니다. 이에 따라 쿠팡과 네이버 쇼핑과 같은 사이트에서 '스트레스 해소 방법'에 대한 검색 결과를 가져오는 방안을 모색했습니다. 여러 시도를 거친 결과, 쇼핑 분야에서는 '영양제'와 '장난감'이라는 빈출 단어만 주로 나온다는 사실을 확인했습니다.

이를 바탕으로 더 신뢰할 수 있는 데이터를 찾기 위해 다른 방법을 구상하였고, 논문에서 얻은 데이터를 활용하여 테스트해보기로 결정했습니다. 특히, 논문 PDF 파일을 저장한 뒤 불필요한 내용을 제거하고, 필요한 특정 페이지만 추출하는 방식으로 데이터를 가공했습니다. 또한, 대부분 의미가 없는 1글자 이하의 단어는 모두 제거하여, 최종적으로 분석 결과를 도출할 수 있었습니다.

---

#### 소감

이번 프로젝트를 통해 다양한 분야의 사람들과 협업하며 많은 것을 배울 수 있었습니다. 혼자서 작업할 때보다, 다른 사람들의 생각을 확인하고 의견을 조율하며 최종 결론에 도달하는 과정이 쉽지 않았지만, 그만큼 더 나은 결과를 얻을 수 있었습니다. 이 과정에서 협업의 중요성을 다시 한 번 느낄 수 있었습니다.

또한, 데이터를 해석하는 과정에서 도메인 지식의 중요성을 깨달았습니다. 충분한 지식 없이 분석을 진행하면 왜곡된 결과에 도달할 위험이 크다는 것도 이번 프로젝트를 통해 확인할 수 있었습니다.  
