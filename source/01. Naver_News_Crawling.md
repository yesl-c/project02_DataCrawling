```python
from bs4 import BeautifulSoup
import time
import selenium
from selenium import webdriver
from random import *
import pandas as pd
import datetime
from datetime import timedelta   # 날짜 사용
```


```python
# test
d = datetime.datetime(2020,6,2)               # 2020년 6월2일
YMD = d.strftime("%Y-%m-%d").replace('-','')  # 2020-06-02에서 -제외하고 문자열로 반환
YMD                                           # 문자열 20200602
```




    '20200602'



## 신문기사 헤드라인 크롤링
* 수집할 웹 페이지 : 네이버 뉴스 정치 속보 전체
* 수집기간 : 2019.09.01 ~ 2020.06.02
* 수집내용 : 기사 헤드라인 (동영상기사 제외)


```python
driver = webdriver.Chrome('chromedriver83.exe')
driver.implicitly_wait(10)
url = 'https://news.naver.com/main/list.nhn?mode=LSD&sid1=100&mid=sec&listType=paper&date=20200602&page=1'
driver.get(url)

article_count = 0      # 한페이지 내 기사수
page = 1               # 페이지 넘기기위한 변수
press_count = []       # 언론사 이름저장
article_contents = []  # 기사제목 저장

while True:
    soup = BeautifulSoup(driver.page_source, 'html.parser')
    article = soup.find_all('a', class_='nclicks(cnt_papaerart1)') # 한 페이지내에 기사제목 전체를 긁어옴
    press = soup.find_all('span', class_='writing')                # 언론사이름
    
    for j in press:
        press_count.append(j.text) # 언론사 이름 press_count에 저장
    
    for i in article: # 기사제목을 하나씩 읽어들임
        article_count += 1               # 기사제목 하나 읽을때마다 변수값이 1씩 증가
        article_contents.append(i.text)  # article_contents에 기사제목 저장
        
    if article_count == 20: # 한페이지를 다읽었으면
        article_count = 0   # 0으로 초기화
        page +=1            # 다음 페이지로 넘어감
        url = 'https://news.naver.com/main/list.nhn?mode=LSD&sid1=100&mid=sec&listType=paper&date='+YMD+'&page='+str(page)
        driver.get(url)
        time.sleep(1)
        soup = BeautifulSoup(driver.page_source,'html.parser')
        
        article2 = soup.find_all('a', class_='nclicks(cnt_papaerart1)') # 다음페이지 정보를 긁어와서
        if article == article2: # 이전페이지와 현재페이지 내용이 같으면 (즉, 그날의 마지막 페이지이면)
            page = 0
            d = d - timedelta(days=1) # 어제 날짜로 변경
            YMD = d.strftime("%Y-%m-%d").replace('-','')
        if YMD == '20190901': # 2019년 9월 1일이 되면
            break             # 데이터 크롤링 종료
            
        time.sleep(2)
    else: # 페이지내에서 읽어온 기사제목이 20개가 안되는 경우 (즉, 마지막 페이지이면 아까와 동일하게)
        article_count = 0
        page +=1
        url = 'https://news.naver.com/main/list.nhn?mode=LSD&sid1=100&mid=sec&listType=paper&date='+YMD+'&page='+str(page)
        driver.get(url)
        time.sleep(1)
        soup = BeautifulSoup(driver.page_source,'html.parser')
        
        article2 = soup.find_all('a', class_='nclicks(cnt_papaerart1)')
        if article == article2: # 마지막 페이지가 맞는지 확실히 검사
            page = 0
            d = d - timedelta(days=1)
            YMD = d.strftime("%Y-%m-%d").replace('-','')
            
            if YMD == '20190901':
                break
                
            time.sleep(2)
```

동영상기사 삭제


```python
def remove_values_from_list(the_list, val): 
    return [value for value in the_list if value != val]

article_contents = remove_values_from_list(article_contents,'동영상기사')
```

수집된 데이터 저장(통합)


```python
dat = {'article':article_contents, 'press':press_count}
df = pd.DataFrame(dat)
df.to_csv('naver.csv', encoding='utf-8')
```

수집된 데이터 저장(언론사 별로)


```python
dict = ['경향신문','국민일보','동아일보','디지털타임스','매일경제','머니투데이','문화일보',
        '서울경제','서울신문','세계일보','아시아경제','이데일리','전자신문','조선일보',
        '중앙일보','파이낸셜뉴스','한겨레','한국경제','한국일보','헤럴드경제']

for i in range dict:    # 언론사 별로 csv파일 저장
    h = df.loc[df['press'] == i]
    h.to_csv(i +'.csv',encoding='utf-8')
```

### 데이터 검증
* 실행시 언론사이름 입력하면 전체 크롤링 자료와 언론사별 분류자료가 일치하는지 확인
* 올바르지 않은 언론사이름 입력시 다시 입력받음
* 끝 입력시 반복문 종료


```python
while True:
    word = input("언론사 이름 입력 : ")
    
    if word in dict:
        b = pd.read_csv(word+'.csv',encoding='utf-8')
        h = df.loc[df['press'] == word]
        h2 = b.loc[:]
        
        if len(h) == len(h2):
            print(len(h) ,'개의 기사가 존재')
            print("검증 완료!")
        elif len(h) != len(h2):
            print("기사 길이가 다릅니다. 검증 실패")
            
    elif word == "끝":
        print("종료!")
        break
        
    else:
        print("정확한 언론사를 입력해주세요!")
```
