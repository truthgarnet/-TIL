## **❓ 오늘의 궁금증**

Json 라이브러리(3 대장) 중에 뭘 사용 해야 할까?

JSON 라이브러리 사용이 필요한 기능을 구축하는 도중, JSON 라이브러리가 다양하다는 것을 알게 되었고, 많이 사용하는 것 3개 중에 어떤 것을 쓰는 것이 가장 좋을 까 고민이 들었다.

## 테스트 과정

1. 라이브러리 추가 (Gradle)

```java
implementation 'com.google.code.gson:gson:2.11.0' // Gson (2024 5 19 마지막 업데이트)
implementation 'com.googlecode.json-simple:json-simple:1.1.1' // Json-simple (2012 3 21 마지막 업데이트)
implementation 'com.fasterxml.jackson.core:jackson-databind:2.17.1' // Jackson (2024 05 05 마지막 업데이트)
```

<br>

2. JSON 파일 받아오기
    
    **준비물: JSON 파일**
    
    ```json
    {
      "entry": [
        {
        ...
    		}, ...
      ],
      "link": [
        {
          "url": "xxx",
          "relation": "self"
        }
      ],
      "id": "ddb28ea3-21ac-4af1-8f13-20b069fdf4f8",
      "type": "transaction-response",
      "resourceType": "Bundle"
    }
    ```
    
    테스트로 진행된 JSON파일은 개인 정보가 담겨 있는 700 줄 이상의 데이터들이 존재한다.
    
    그중에서 id만 가져오는 테스트를 진행했다.
    
    **2-1. Json-Simple**
    
    ```java
    @Slf4j
    public class JsonSimpleUtil {
    
        // 파일 읽기
        public static String getJsonData(String absoultePath) {
            String id = "";
            JSONParser parser = new JSONParser();
            try (Reader reader = new FileReader(absoultePath)) {
    
                JSONObject jsonObject = (JSONObject) parser.parse(reader);
                System.out.println(jsonObject);
    
                id = (String) jsonObject.get("id");
                System.out.println(id);
    
            } catch (IOException e) {
                log.error("Json 파일 불러오기 실패 {}", e);
                e.printStackTrace();
            } catch (ParseException e) {
                log.error("Json 파일 불러오기 실패 {}", e);
                throw new RuntimeException(e);
            }
            return id;
        }
    }
    ```
    
    **2-2. Gson**
    
    ```java
    public class GsonUtil {
    
        public static String getJsonData(String absoultePath) {
            String id = "";
            try {
                // 1. Json 파일 불러오기
                Reader reader = new FileReader(absoultePath);
    
                // 2. Gson 객체 생성
                Gson gson = new Gson();
                JsonObject jsonObject = gson.fromJson(reader, JsonObject.class);
    
                id = jsonObject.get("id").toString();
            } catch (FileNotFoundException e) {
                log.error("Json 파일 불러오기 실패 {}", e);
            }
    
            return id;
        }
    }
    ```
    
    **2-3. JackSon**
    
    ```java
    @Slf4j
    public class JacksonUtil {
    
        public static String getJsonData(String absoultePath) {
            ObjectMapper objectMapper = new ObjectMapper();
            String id = "";
            try {
                JsonNode jsonNode = objectMapper.readTree(new File(absoultePath));
                id = jsonNode.get("id").asText();
            } catch (IOException e) {
                log.error("Json 파일 불러오기 실패 {}", e);
                throw new RuntimeException(e);
            }
    
            return id;
        }
    }
    ```
    
<br>

## 결과

![Untitled](/Images/result.png)

인터넷에 찾아본 결과, 큰 JSON 파일에서 성능은 Jackson이 가장 좋다 하고, 작은 JSON 파일에서는 Gson의 성능이 가장 좋다고 한다.

<br>

## 정리

![Untitled](/Images/ranking.png)

![Untitled](/Images/ranking2.png)

사용량은 [Maven Respoitory](https://mvnrepository.com/open-source/json-libraries)에서 확인할 수 있다.

|  | 큰 파일 | 작은 파일 | 사용량 |
| --- | --- | --- | --- |
| 1 | Jackson | Gson | Jackson |
| 2 | Json-Simple | Json-Simple | Gson |
| 3 | Gson | Jackson | Json-Simple |

700 줄 정도 되면, 작은 JSON 파일인지 아닌지, 확인 하고 싶어서 확인한 결과, 700 줄, 5KB 정도 되는 JSON 파일은 작은 JSON 파일 축에 들어가는 것을 알 수 있다. 하지만, 파일이 더 큰 경우도 들어올 수 있기 때문에, 파일의 크기가 골고루 들어왔을 때, 무난한 성능을 보이는 JSON-SIMPLE를 사용하는 것이 좋다고 생각이 들었다.