# 지능화 기술 생태계 분석을 위한 데이터 수집 및 가공 (Data collection and processing for intelligent technology ecosystem analysis)

## Packages
1. sklearn(TfidfVectorizer, CountVectorizer, PCA)
2. KMeans
3. DBSCAN!
4. 
5. 
6. 

## Index
- Research Proceduer
  * Item-based Technology Type Analysis
  * Topic-based Technology Type Analysis
- Research Result (Github's key Repository Analysis)
  * Star-based
  * Big Tech Company
  * Future Tech : Autonomous-vehicle, Metaverse


## 연구 절차 (Research Proceduer)
[Uploading 스크린샷 2022-03-02 오후 3.26.33.png…]()

1. 데이터 수집 및 전처리

  1) 깃허브 오픈소스 정보 및 API 분석

  - 깃허브는 대용량의 오픈소스 정보를 효과적으로 확인하기 위한 도구로 API를 제공하고 있으며, 개발자, 개발환경, 현황 등 여러 기술속성을 검색할 수 있도록 지원하고 있음.
  - 아래 url을 통해 "deep learning" 키워드 검색 결과에 대한 정보를 웹페이지 상에서 API 형태로 확인 가능
  https://api.github.com/search/repositories?q=deep%20learning&page,per_page,sort,order
  
  <img width="1273" alt="스크린샷 2022-03-02 오후 3 47 13" src="https://user-images.githubusercontent.com/87521259/156309578-501ba7dd-f673-44b4-8742-2ab95e4c2e00.png">

  2) API 이용하여 필요한 저장소 정보 Crawling
  ```
  def topic(t):
    
    topic = t.replace(' ', '%20')
    response = urlopen('https://api.github.com/search/repositories?q={}&page,per_page,sort,order'.format(topic)).read().decode('utf-8')

    responseJson = json.loads(response)

    name_lst = []
    type_lst = []
    create_lst = []
    size_lst = []
    star_lst = []
    fork_lst = []
    login_lst = []

    items = responseJson.get('items')

    for lst in items:
        name = lst.get('name')
        typ = lst.get('owner').get('type')
        create = lst.get('created_at')
        size = lst.get('size')
        star = lst.get('stargazers_count')
        fork = lst.get('forks_count')
        login = lst.get('owner').get('login')

        name_lst.append(name)
        type_lst.append(typ)
        create_lst.append(create)
        size_lst.append(size)
        star_lst.append(star)
        fork_lst.append(fork)
        login_lst.append(login)

#         print('{} / {} / {} / {} / {} / {} / {}'.format(name, typ, create, size, star, fork, login))

    df = pd.DataFrame([name_lst, type_lst, create_lst, size_lst, star_lst, fork_lst, login_lst])
    df = df.transpose()
    df.columns = ['name','type','created_at','size','stargazers_count','fork','login']
    return df
 
 # test
topic('deep learning')
 ```
 <img width="852" alt="스크린샷 2022-03-02 오후 3 48 58" src="https://user-images.githubusercontent.com/87521259/156309815-3d802eca-173e-4956-a84f-0e02e350bb27.png">

 
