```python
!apt-get update -qq
!apt-get install fonts-nanum* -qq 
```

    Selecting previously unselected package fonts-nanum.
    (Reading database ... 144379 files and directories currently installed.)
    Preparing to unpack .../fonts-nanum_20170925-1_all.deb ...
    Unpacking fonts-nanum (20170925-1) ...
    Selecting previously unselected package fonts-nanum-eco.
    Preparing to unpack .../fonts-nanum-eco_1.000-6_all.deb ...
    Unpacking fonts-nanum-eco (1.000-6) ...
    Selecting previously unselected package fonts-nanum-extra.
    Preparing to unpack .../fonts-nanum-extra_20170925-1_all.deb ...
    Unpacking fonts-nanum-extra (20170925-1) ...
    Selecting previously unselected package fonts-nanum-coding.
    Preparing to unpack .../fonts-nanum-coding_2.5-1_all.deb ...
    Unpacking fonts-nanum-coding (2.5-1) ...
    Setting up fonts-nanum-extra (20170925-1) ...
    Setting up fonts-nanum (20170925-1) ...
    Setting up fonts-nanum-coding (2.5-1) ...
    Setting up fonts-nanum-eco (1.000-6) ...
    Processing triggers for fontconfig (2.12.6-0ubuntu2) ...
    


```python
!pip install konlpy
```

    Collecting konlpy
    [?25l  Downloading https://files.pythonhosted.org/packages/85/0e/f385566fec837c0b83f216b2da65db9997b35dd675e107752005b7d392b1/konlpy-0.5.2-py2.py3-none-any.whl (19.4MB)
    [K     |████████████████████████████████| 19.4MB 1.3MB/s 
    [?25hRequirement already satisfied: lxml>=4.1.0 in /usr/local/lib/python3.6/dist-packages (from konlpy) (4.2.6)
    Collecting colorama
      Downloading https://files.pythonhosted.org/packages/c9/dc/45cdef1b4d119eb96316b3117e6d5708a08029992b2fee2c143c7a0a5cc5/colorama-0.4.3-py2.py3-none-any.whl
    Collecting beautifulsoup4==4.6.0
    [?25l  Downloading https://files.pythonhosted.org/packages/9e/d4/10f46e5cfac773e22707237bfcd51bbffeaf0a576b0a847ec7ab15bd7ace/beautifulsoup4-4.6.0-py3-none-any.whl (86kB)
    [K     |████████████████████████████████| 92kB 7.7MB/s 
    [?25hCollecting JPype1>=0.7.0
    [?25l  Downloading https://files.pythonhosted.org/packages/2d/9b/e115101a833605b3c0e6f3a2bc1f285c95aaa1d93ab808314ca1bde63eed/JPype1-0.7.5-cp36-cp36m-manylinux2010_x86_64.whl (3.6MB)
    [K     |████████████████████████████████| 3.6MB 45.1MB/s 
    [?25hCollecting tweepy>=3.7.0
      Downloading https://files.pythonhosted.org/packages/36/1b/2bd38043d22ade352fc3d3902cf30ce0e2f4bf285be3b304a2782a767aec/tweepy-3.8.0-py2.py3-none-any.whl
    Requirement already satisfied: numpy>=1.6 in /usr/local/lib/python3.6/dist-packages (from konlpy) (1.18.5)
    Requirement already satisfied: requests>=2.11.1 in /usr/local/lib/python3.6/dist-packages (from tweepy>=3.7.0->konlpy) (2.23.0)
    Requirement already satisfied: six>=1.10.0 in /usr/local/lib/python3.6/dist-packages (from tweepy>=3.7.0->konlpy) (1.12.0)
    Requirement already satisfied: requests-oauthlib>=0.7.0 in /usr/local/lib/python3.6/dist-packages (from tweepy>=3.7.0->konlpy) (1.3.0)
    Requirement already satisfied: PySocks>=1.5.7 in /usr/local/lib/python3.6/dist-packages (from tweepy>=3.7.0->konlpy) (1.7.1)
    Requirement already satisfied: certifi>=2017.4.17 in /usr/local/lib/python3.6/dist-packages (from requests>=2.11.1->tweepy>=3.7.0->konlpy) (2020.6.20)
    Requirement already satisfied: idna<3,>=2.5 in /usr/local/lib/python3.6/dist-packages (from requests>=2.11.1->tweepy>=3.7.0->konlpy) (2.9)
    Requirement already satisfied: chardet<4,>=3.0.2 in /usr/local/lib/python3.6/dist-packages (from requests>=2.11.1->tweepy>=3.7.0->konlpy) (3.0.4)
    Requirement already satisfied: urllib3!=1.25.0,!=1.25.1,<1.26,>=1.21.1 in /usr/local/lib/python3.6/dist-packages (from requests>=2.11.1->tweepy>=3.7.0->konlpy) (1.24.3)
    Requirement already satisfied: oauthlib>=3.0.0 in /usr/local/lib/python3.6/dist-packages (from requests-oauthlib>=0.7.0->tweepy>=3.7.0->konlpy) (3.1.0)
    Installing collected packages: colorama, beautifulsoup4, JPype1, tweepy, konlpy
      Found existing installation: beautifulsoup4 4.6.3
        Uninstalling beautifulsoup4-4.6.3:
          Successfully uninstalled beautifulsoup4-4.6.3
      Found existing installation: tweepy 3.6.0
        Uninstalling tweepy-3.6.0:
          Successfully uninstalled tweepy-3.6.0
    Successfully installed JPype1-0.7.5 beautifulsoup4-4.6.0 colorama-0.4.3 konlpy-0.5.2 tweepy-3.8.0
    


```python
import nltk
from konlpy.tag import Kkma
from konlpy.tag import Hannanum
from wordcloud import WordCloud, STOPWORDS
from PIL import Image

import matplotlib.pyplot as plt
import matplotlib.font_manager as fm

import numpy as np
import matplotlib as mpl
```


```python
path = '/usr/share/fonts/truetype/nanum/NanumGothicEco.ttf'
font_name = fm.FontProperties(fname=path, size=10).get_name()
print(font_name)
plt.rc('font', family=font_name)
mpl.rcParams['axes.unicode_minus'] = False   # 음수 표시
```

    NanumGothic Eco
    


```python
fm._rebuild()
```

파일 불러오기


```python
doc_ko = open('조선일보.txt').read()
```

워드클라우드용 마스크 설정


```python
mask = np.array(Image.open("셜록.jpg"))
```


```python
plt.figure(figsize=(15,8))
plt.imshow(mask,cmap=plt.cm.gray, interpolation='bilinear')
plt.axis('off')
plt.show()
```


![png](output_9_0.png)


워드클라우드 생성


```python
# OKT 클래스를 이용한 명사확인
from konlpy.tag import Okt
t = Okt()
doc_nouns = t.nouns(doc_ko)

ko = nltk.Text(doc_nouns, name="조선일보")
# print(type(ko))
# print(len(ko.tokens))

stop_words = ['것','이','고','전','연','군','의','수','등','비','안','명','선','중','때문',
              '경향신문','국민일보','동아일보','디지털타임스','매일경제','머니투데이',
              '문화일보','서울경제','서울신문','세계일보','아시아경제','이데일리','전자신문',
              '조선일보','중앙일보','파이낸셜뉴스','한겨레','한국경제','한국일보','헤럴드경제',
              '오늘','뉴스','종합','속보','단독','선택','포토','미아','인터뷰']   # 불용어 사전
new_ko = [ ]        # 가용어 사전
for one_word in ko:
  if one_word not in stop_words:  # 불용어가 아닌 것
    new_ko.append(one_word)       # 추가
new_ko = nltk.Text(new_ko, name='조선일보')

# 빈도수 높은 단어 1000개 추출
data2 = new_ko.vocab().most_common(1000) 

# 워드 클라우드 표현을 위한 데이터 생성
wc = WordCloud(background_color='white',
    max_words=1000,
    mask=mask,
    contour_width=0,
    font_path=path).generate_from_frequencies(dict(data2))   # 불용어 빼고 단어의 빈도로

plt.figure(figsize=(20,20))
plt.imshow(wc)
plt.axis("off")
plt.show()
```


![png](output_11_0.png)


단어들의 사용 횟수 확인 (빈도분석)


```python
most_fre = new_ko.vocab().most_common(50)
most_fre
```




    [('한국', 293),
     ('대통령', 267),
     ('통합', 140),
     ('정부', 126),
     ('비례', 123),
     ('조국', 117),
     ('총선', 109),
     ('출마', 100),
     ('공천', 98),
     ('김정은', 98),
     ('민주당', 93),
     ('황교안', 88),
     ('트럼프', 82),
     ('검찰', 71),
     ('의원', 67),
     ('선거', 67),
     ('국회', 65),
     ('수사', 62),
     ('때', 58),
     ('국민', 58),
     ('안철수', 57),
     ('코로나', 56),
     ('우리', 55),
     ('논란', 55),
     ('선거법', 54),
     ('당', 53),
     ('대표', 53),
     ('정치', 52),
     ('총리', 52),
     ('이해찬', 51),
     ('땐', 50),
     ('방위', 50),
     ('보수', 50),
     ('석', 49),
     ('은', 48),
     ('인사', 47),
     ('듯', 46),
     ('또', 46),
     ('이낙연', 46),
     ('말', 45),
     ('유승민', 45),
     ('후보', 45),
     ('의혹', 44),
     ('위', 44),
     ('제', 44),
     ('김종인', 43),
     ('장관', 43),
     ('불', 43),
     ('더', 43),
     ('정권', 43)]




```python
plt.figure(figsize=(12,6))
ko.plot(50)
plt.show()
```


![png](output_14_0.png)


신문사 별 기사수, 단어수 시각화


```python
import pandas as pd
import seaborn as sns

df = pd.read_csv('기사수2.csv', encoding='cp949')
df = df.iloc[:20,:]

plt.figure(figsize=(16,6))
plt.subplot(1,2,1)
plt.title('신문사 별 기사수')
df.sort_values(by=['기사수'], ascending=True, axis=0, inplace=True)
sns.barplot(x='신문사', y='기사수', data=df)
plt.xticks(rotation=50)

plt.subplot(1,2,2)
plt.title('신문사 별 단어수')
df.sort_values(by=['단어수'], ascending=True, axis=0, inplace=True)
sns.barplot(x='신문사', y='단어수', data=df)
plt.xticks(rotation=50)
```




    (array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14, 15, 16,
            17, 18, 19]), <a list of 20 Text major ticklabel objects>)




![png](output_16_1.png)


신문사 별 민주당 vs 미래당(통합당) 언급 수에 따른 scatter plot
* 진보 : 민주당
* 보수 : 미래당, 통합당


```python
dat = pd.read_csv('민주vs미래.csv', encoding='cp949')
dat['total'] = dat.민주 + dat.미래
# label = dat.신문사.to_list()

plt.figure(figsize=(12,10))
plt.xlim(0,350)
plt.ylim(0,350)
sns.scatterplot(x='민주', y='미래', hue='신문사', alpha=.7, data=dat)

for i in range(20):
  plt.text(dat.민주[i], dat.미래[i], dat.신문사[i])

plt.legend(loc='upper right')

plt.show()
```


![png](output_18_0.png)



```python

```
