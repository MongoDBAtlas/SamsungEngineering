<img src="https://companieslogo.com/img/orig/MDB_BIG-ad812c6c.png?t=1648915248" width="50%" title="Github_Logo"/> <br>


# Samsung Engineering MongoDB Atlas Data API Handon

### [&rarr; App Service](#AppService)
### [&rarr; App Service Configuration](#Configuration)
### [&rarr; App Service Authentication](#Authentication)
### [&rarr; DATA API CRUD](#CRUD)
<br>

### AppService
Atlas Data API는 App Service 중 하나입니다. 실습을 위하여 Atlas cluster가 필요 하며 기본 데이터를 로드 하여 진행합니다.   

테스트를 위해 App Service를 생성합니다. Data API를 생성 하면 자동으로 App Service 가 생성 됩니다. Handson에서는 Data API를 이용하여 생성 합니다.   


메뉴중 Data API를 클릭 합니다. 기본적으로 대상 데이터 베이스클러스터를 선택 하여야 합니다. 테스트를 진행할 클러스터를 선택 하여 줍니다.

<img src="/images/image01.png" width="60%" height="60%">     

선택 후 enable Data API를 클릭합니다.  

Data API의 기본 인증 방법은 API key를 이용한 방법 입니다. 따라서 바로 API key 생성 메뉴가 나오게 됩니다.

키이름을 입력 하고 Generate API Key 버튼을 클릭 합니다. 

<img src="/images/image02.png" width="90%" height="90%">     

API key는 한번만 볼 수 있음으로 별도 보관을 하여 줍니다. 

완료 화면에서는 테스트 할 수 있는 주소가 보여 지게 됩니다. 해당 Endpoint를 이용하여 수출 테스트를 진행 합니다.

<img src="/images/image03.png" width="90%" height="90%">     

다음은 Sample_mflix 데이터베이스에 있는 movies 컬렉션을 대상으로하여 데이터 1건을 쿼리 하는 API 호출 입니다.

````
% curl --location --request POST 'https://us-west-2.aws.data.mongodb-api.com/app/data-nwywi/endpoint/data/v1/action/findOne' \
--header 'Content-Type: application/json' \
--header 'Access-Control-Request-Headers: *' \
--header 'api-key: dN3cIcBAWuD8SBMzjqn9zp*********' \
--data-raw '{
    "collection":"movies",
    "database":"sample_mflix",
    "dataSource":"Serverless",
    "projection": {"_id": 1}
}'

{"document":{"_id":"573a1390f29313caabcd4135"}}%                                                                                                    kyudong.kim@Kyudongui-MacBookPro ~ % 
````

최종 Data API 화면은 다음과 같습니다.

<img src="/images/image04.png" width="90%" height="90%">     


### Configuration

App Services 탭에 보면 data라는 이름으로 app service 가 한개 생성 되어 있습니다. 이를 클릭 하면 상세 내용을 볼 수 있습니다.

<img src="/images/image05.png" width="90%" height="90%">     

메뉴 중 app Settings를 클릭 하면 Data API가 서비스 되는 지역 정보를 볼 수 있습니다.

<img src="/images/image06.png" width="90%" height="90%">     

Single Region에서 서비스 되는 것을 Global로 변경 하여 줍니다.     

<img src="/images/image07.png" width="90%" height="90%">     

Region 변경에는 수분이 소요 되며 다시 Single region으로 전환 되지 않습니다. Global의 경우 모든 Edge에 Data API가 배치 되어 client와 가장 가까운 쪽으로 자동으로 라우팅이 됩니다. 기존 end point는 리전에 관련된 정보가 url에 포함되어 있으나 global로 변경하면 주소가 변경됩니다.    

테스트를 위해 Data API 페이지에서 Test User API 버튼을 클릭 하여 줍니다.   

표시된 주소로 API 를 호출 하여 줍니다.

````
~ % curl --location --request POST 'https://data.mongodb-api.com/app/data-nwywi/endpoint/data/v1/action/findOne' \
--header 'Content-Type: application/json' \
--header 'Access-Control-Request-Headers: *' \
--header 'api-key: dN3cIcBAWuD8SBMzjqn9zpZ1gt8C********' \
--data-raw '{
    "collection":"movies",
    "database":"sample_mflix",
    "dataSource":"Serverless",
    "projection": {"_id": 1}
}'

{"document":{"_id":"573a1390f29313caabcd4135"}}%                                                                                                    

````

기본 주소가 기존 https://us-west-2.aws.data.mongodb-api.com 에서 https://data.mongodb-api.com 으로 변경된 것을 볼 수 있습니다. 

### Authentication

API를 사용하기 위해 호출자 인증은 보안을 위해 중요한 부분입니다. 인증 방법은 기본 API Key가 설정 되며 이외에 email, 3rd party 인증 등이 가능합니다.    
Email/Password를 통한 인증을 테스트 합니다. 
먼저 Email을 이용한 인증을 허가해 주어야 합니다. App Services 메뉴 중 Authentication 메뉴에서 Authentication Providers를 선택 합니다.   
사용 가능한 인증 방법 리스트 들이 있으며 기본 API Key만 enabled 되어 있습니다. Email/Password를 Edit 버튼을 클릭합니다.   

<img src="/images/image09.png" width="90%" height="90%">     

이메일 패스워드 처리를 위한 설정이 필요합니다. 사용자 초대 후 처리 방법을 function 혹은 webservice등을 이용할 수 있습니다. Handson에서는 자동 처리로 합니다.   

<img src="/images/image10.png" width="90%" height="90%">     

설정이 완료 되었음으로 Email 사용자를 추가 하여 줍니다.   

사용자 추가를 위해 App Users 메뉴에서 Add New User를 합니다.

<img src="/images/image11.png" width="90%" height="90%">     

Email 과 Password를 입력 하여 사용자를 추가 합니다.

<img src="/images/image12.png" width="90%" height="90%">     

사용자가 추가 되었으며 상태가 Confirmed 상태 임으로 이를 이용하여 Data API를 호출 테스트 합니다.   

<img src="/images/image13.png" width="90%" height="90%">     

기존 API Key를 이용한 부분을 입력 해준 Email/password 로 변경 하고 호출 합니다.   

````
~ % curl --location --request POST 'https://data.mongodb-api.com/app/data-nwywi/endpoint/data/v1/action/findOne' \
--header 'Content-Type: application/json' \
--header 'Access-Control-Request-Headers: *' \
--header 'email: kyudong.kim@mongodb.com' \
--header 'password: ******' \
--data-raw '{
    "collection":"movies",
    "database":"sample_mflix",
    "dataSource":"Serverless",
    "projection": {"_id": 1}
}'
{"document":{"_id":"573a1390f29313caabcd4135"}}%         

````

### CRUD

단일 문서를 Find 합니다. 

moives 에서 1993년도 영화 1편을 출력 합니다.    

````
~ % curl --location 'https://data.mongodb-api.com/app/data-nwywi/endpoint/data/v1/action/findOne' \
--header 'Content-Type: application/json' \
--header 'Access-Control-Request-Headers: *' \
--header 'email: kyudong.kim@mongodb.com' \
--header 'password: ****' \
--data '{"collection": "movies", "database":"sample_mflix", "dataSource":"Serverless", "filter": {"year":1993}}'

{"document":{"_id":"573a1399f29313caabcecc19","plot":"Jude, a college literature professor, falls for one of his students. She is more interested in the empirical experience of a relationship with a man whose life is ruled by the themes of the...","genres":["Comedy","Drama","Romance"],"runtime":53,"cast":["Martin Donovan","Matt Malloy","Rebecca Nelson","Julie Kessler"],"num_mflix_comments":0,"poster":"https://m.media-amazon.com/images/M/MV5BMTU2MjU5OTc2NF5BMl5BanBnXkFtZTcwNjk4NDAyMQ@@._V1_SY1000_SX677_AL_.jpg","title":"Surviving Desire","fullplot":"Jude, a college literature professor, falls for one of his students. She is more interested in the empirical experience of a relationship with a man whose life is ruled by the themes of the Russian Lit. he extolls in class. Jude shows an interesting side of the stigmas associated with transgenerational relationships and how to deal with the inevitable pain of a love doomed to failure.","languages":["English"],"released":"1993-11-26T00:00:00Z","directors":["Hal Hartley"],"writers":["Hal Hartley"],"awards":{"wins":1,"nominations":0,"text":"1 win."},"lastupdated":"2015-08-12 00:22:48.533000000","year":1993,"imdb":{"rating":7.5,"votes":1137,"id":103010},"countries":["USA"],"type":"movie","tomatoes":{"viewer":{"rating":3.8,"numReviews":1149,"meter":89},"dvd":"2002-04-09T00:00:00Z","production":"Wellspring Media Inc.","lastUpdated":"2015-09-10T19:56:34Z"}}}     

````

Postman 을 이용하여 호출 하면 다음과 같이 결과를 볼 수 있습니다.   

<img src="/images/image14.png" width="90%" height="90%">     

2000 년대 이후의 영화로 제목으로 정렬하여 20개를 가져오도록 합니다.

````
db.movies.find({year:{$gte: 2000}}).sort({title:1}).limit(20)

````

Postman을 이용한 호출은 다음과 같습니다. findOne 대신 find를 사용하고 filter, sort, limit 조건을 입력 하여 줍니다.   

<img src="/images/image15.png" width="90%" height="90%">  

Curl을 이용한 호출은 다음과 같습니다.   

````
~ % curl --location 'https://data.mongodb-api.com/app/data-nwywi/endpoint/data/v1/action/find' \
--header 'Content-Type: application/json' \
--header 'Access-Control-Request-Headers: *' \
--header 'email: kyudong.kim@mongodb.com' \
--header 'password: ******' \
--data '{"collection": "movies", "database":"sample_mflix", "dataSource":"Serverless", "filter": {"year":{"$gte": 2000}}, "sort": {"title":1}, "limit":20}'

{"documents":[{"_id":"573a13cef29313caabd86ecc","plot":"Through intimate interviews, provocative art, and rare, historical film and video footage, this feature documentary reveals how art addressing political consequences of discrimination and ...","genres":["Documentary"],"runtime":83,"metacritic":70,"cast":["Lynn Hershman-Leeson"],"num_mflix_comments":1,"poster":"https://m.media-amazon.com/images/M/MV5BMjE1MDU1MDA2Nl5BMl5BanBnXkFtZTcwNTQ2Mzk2NQ@@._V1_SY1000_SX677_AL_.jpg","title":"!Women Art Revolution","fullplot":"Through intimate interviews, provocative art, and rare, historical film and video footage, this feature documentary reveals how art addressing political consequences of discrimination and violence, the Feminist Art Revolution radically transformed the art and culture of our times.","languages":["English"],"released":"2011-06-01T00:00:00Z","directors":["Lynn Hershman-Leeson"],"awards":{"wins":0,"nominations":2,"text":"2 nominations."},"lastupdated":"2015-04-02 00:54:39.997000000","year":2010,"imdb":{"rating":6.7,"votes":142,"id":1699720},"countries":["USA"],"type":"movie","tomatoes":{"website":"http://www.womenartrevolution.com/","viewer":{"rating":3.7,"numReviews":203,"meter":60},"dvd":"2012-03-19T00:00:00Z","critic":{"rating":6.9,"numReviews":24,"meter":83},"boxOffice":"$51.5k","consensus":"Though a tad messy in spots, !Women Art Revolution is a worthy chronicle of an influential art movement seldom explored or documented.","rotten":4,"production":"Zeitgeist Films","lastUpdated":"2015-07-16T18:12:36Z","fresh":20}},{"_id":"573a13e6f29313caabdc56c7","plot":"From her childhood bedroom in the Chicago suburbs, an American teenage girl uses social media to run the revolution in Syria. Armed with Facebook, Twitter, Skype and cameraphones, she helps...","genres":["Documentary","Action","Drama"],"runtime":74,"title":"#chicagoGirl: The Social Network Takes on a Dictator","poster":"https://m.media-amazon.com/images/M/MV5BMTgwODIxMjM3NV5BMl5BanBnXkFtZTgwMzYzODA2NjE@._V1_SY1000_SX677_AL_.jpg","countries":["USA","Syria"],"fullplot":"From her childhood bedroom in the Chicago suburbs, an American teenage girl uses social media to run the revolution in Syria. Armed with Facebook, Twitter, Skype and cameraphones, she helps her social network in Damascus and Homs braves snipers and shelling in the streets and the world the human rights atrocities of one of the most brutal dictators. But as the revolution rages on, everyone in the network must decide what is the most effective way to fight a dictator: social media or AK-47s.","languages":["English"],
...
````

데이터를 생성 하여 봅니다. 새로운 컬렉션에 데이터를 생성 하도록 합니다.   

다음과 같이 samsung 데이터베이스에 users 컬렉션에 사용자 정보를 생성 하여 줍니다. 생성되는 Json은 다음과 같습니다.  

````
{
        ssn:"123-456-0001", 
        email:"user@email.com", 
        name:"Gildong Hong", 
        DateOfBirth: "1st Jan.", 
        Hobbies:["Martial arts"],
        Addresses:[{"Address Name":"Work","Street":"431, Teheran-ro GangNam-gu ","City":"Seoul", "Zip":"06159"}], 
        Phones:[{"type":"mobile","number":"010-5555-1234"}]
      }
````

Postman 에서 호출은 다음과 같습니다. Endpoint를 insertOne으로 변경하고 body 중 document 항목에 생성할 문서를 입력 하여 줍니다.   

<img src="/images/image16.png" width="90%" height="90%">  

Curl을 이용한 호출 주소는 다음과 같습니다.  

````
~ % curl --location 'https://data.mongodb-api.com/app/data-nwywi/endpoint/data/v1/action/insertOne' \
--header 'Content-Type: application/json' \
--header 'Access-Control-Request-Headers: *' \
--header 'email: kyudong.kim@mongodb.com' \
--header 'password: ***' \
--data-raw '{"collection": "users", "database":"samsung", "dataSource":"Serverless", "document": {
        "ssn":"123-456-0001",
        "email":"user@email.com",
        "name":"Gildong Hong",
        "DateOfBirth": "1st Jan.",
        "Hobbies":["Martial arts"],
        "Addresses":[{"Address Name":"Work","Street":"431, Teheran-ro GangNam-gu ","City":"Seoul", "Zip":"06159"}],
        "Phones":[{"type":"mobile","number":"010-5555-1234"}]
      }
}'

{"insertedId":"64a94b3bb8dc13ed68a69cb3"}

````

데이터를 업데이트 하도록 합니다. 
ssn 으로 검색하고 email 을 변경 하도록 합니다.   

````
db.users.updateOne({"ssn":"123-456-0001"},{$set:{email:"gildong@email.com"}} )
````

Postman을 이용한 호출은 다음과 같습니다. filter 항목에 조회 조건을 생성 하여 주고 update 항목에 수정되는 내용을 추가 하여 줍니다.   

<img src="/images/image17.png" width="90%" height="90%">  

````
 ~ % curl --location 'https://data.mongodb-api.com/app/data-nwywi/endpoint/data/v1/action/updateOne' \
--header 'Content-Type: application/json' \
--header 'Access-Control-Request-Headers: *' \
--header 'email: kyudong.kim@mongodb.com' \
--header 'password: ***' \
--data-raw '{"collection": "users", "database":"samsung", "dataSource":"Serverless", "filter": {"ssn":"123-456-0001"},
        "update": {"$set":{"email":"gildong@email.com"}}
      }'
{"matchedCount":1,"modifiedCount":1}
````

Aggregate을 이용하여 데이터를 조회 합니다. ssn 으로 group을 하여 숫자를 count 하는 간단한 aggregation pipeline을 작성합니다.   

````
samsung> db.users.aggregate([{$group: {_id:"$ssn", "count":{$sum:1}}}])
[ { _id: '123-456-0001', count: 1 } ]
````

Postman을 이용한 호출은 다음과 같습니다. pipeline 항목에 Aggregation 관련 내용을 작성하여 줍니다.   

<img src="/images/image18.png" width="90%" height="90%">  

Curl을 이용한 호출은 다음과 같습니다.  

````
 % curl --location 'https://data.mongodb-api.com/app/data-nwywi/endpoint/data/v1/action/aggregate' \
--header 'Content-Type: application/json' \
--header 'Access-Control-Request-Headers: *' \
--header 'email: kyudong.kim@mongodb.com' \
--header 'password: ****' \
--data '{"collection": "users", "database":"samsung", "dataSource":"Serverless", "pipeline": [{"$group": {"_id":"$ssn", "count":{"$sum":1}}}]}'

{"documents":[{"_id":"123-456-0001","count":1}]}
````

생성한 내용을 API를 이용하여 삭제 합니다.  

````
samsung> db.users.deleteOne({"ssn":"123-456-0001"})
````

Postman을 이용한 호출은 다음과 같습니다. filter 항목에 삭제에 대한 조건을 입력 하여 줍니다.   

<img src="/images/image19.png" width="90%" height="90%">  

Curl을 이용한 삭제는 다음과 같습니다. 
````
~ % curl --location 'https://data.mongodb-api.com/app/data-nwywi/endpoint/data/v1/action/deleteOne' \
--header 'Content-Type: application/json' \
--header 'Access-Control-Request-Headers: *' \
--header 'email: kyudong.kim@mongodb.com' \
--header 'password: ****' \
--data '{"collection": "users", "database":"samsung", "dataSource":"Serverless", "filter": {
        "ssn":"123-456-0001"}
}'
{"deletedCount":1}

````

