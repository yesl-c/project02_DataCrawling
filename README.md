# project02_DataCrawling
Analysis of Political Propensity by Newspaper through Web Data Crawling

---

## 신문사 별 정치면 헤드라인을 통한 정치성향 분석
**(1) 신문기사 헤드라인 크롤링**
  * 수집할 웹 페이지 : [네이버 뉴스 정치 속보 전체](https://news.naver.com/main/list.nhn?mode=LSD&mid=sec&sid1=100)
  * 수집기간 : 2019.09.01 ~ 2020.06.02
  * 수집내용 : 기사 헤드라인 (동영상기사 제외)
  
**(2) 데이터 설명**
  * 20개의 신문사
  * 정치지면의 헤드라인
  * 신문사 별 주요 단어 추출 (불용어를 제외하고 빈도 높은 순으로)
  
**(3) 분석 방법**
  * 빈도분석
  * 워드클라우드로 시각화
    + 신문사 별 성향을 파악하는 단어를 추출하기 위해 상위 5개의 단어는 제외해서 워드클라우드 생성 후 비교
    + 가장 많이 사용된 단어들은 신문사 별로 차이가 별로 없기 때문 (공통된 단어들이 많음)
  * 신문사 별 정치성향을 드러내는 단어(민주당, 미래당/통합당)으로 정치성향 파악 (scatter plot으로 시각화)
  
**(4) 활용 라이브러리**
  * 데이터 수집 : BeautifulSoup, selenium
  * 워드클라우드 : wordcloud, Okt
  * 그래프 : matplotlib, seaborn
  
**(5) Insight**
  * 일부 언론사의 경우 일제강점기부터 비롯된 뚜렷한 정치적 성향을 띈 언론사와 광복 이후 출현한 언론사는 일부 민족지도 있었으나 군부에 의해 통폐합을 반복하며 보수적
  * 거론된 20 개 언론사는 보수적 성향의 지향점을 가질 수 밖에 없는 태생적 한계가 있음
  * 모기업 언론사의 경우 기업의 입장과 함께 모기업 관련 사업에 따른 방어기제가 바탕에 깔려 있어 이슈 키워드 활용의 관점이 각기 달리 확인됨


  * 종교재단 : 국민일보, 세계일보
  * 개인사주 : 동아일보, 조선일보 , 경향신문(MBC), 중앙일보(TBC)
  * 모기업 :  헤럴드경제, 한국일보
  * 정부기관 : 서울신문
  * 사주모집 : 한겨레
  * 온라인 : 종합지
 
 
