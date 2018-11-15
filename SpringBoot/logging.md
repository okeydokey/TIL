# logging



#### spring boot application.yml 에 따라 logback-spring.xml에서 log path를 지정하고 싶을 경우

- logback-spring.xml 에 Path를 아래와 같이 지정

  - ```xml
    ${LOG_PATH:-.}
    ```

    - :-. < 기본값 지정을 안할 경우 spring boot 설정이 적용되기 전에 LOG_PATH 값을 불러오지 못해 LOG_PATH_IS_UNDEFINED 이름의 폴더가 생성됨

- application.yml

  - ```yml
    logging:
      path: ../logs
    ```

