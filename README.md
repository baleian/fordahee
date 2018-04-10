
1. 프로젝트 경로에서 docker-compose up 명령어로 컨테이너를 생성 + 실행한다. 
  (컨테이너 종료: ctrl + C, 컨테이너 완전 삭제: docker-compose down)
  
2. mysql1 (localhost:33306), mysql2 (localhost:43306) 들어가서 각각 sql 폴더 밑에 있는 쿼리를 실행하여
  스키마 및 테이블을 생성한다
  
3. localhost:8080/nifi 접속하여 템플릿 업로드 후 생성 한다

4. DBtoDB 설명:
  - controller service 에 있는 dbcp1, dbcp2 에 각각 디비에 맞는 패스워드 설정 후 실행 필요 (server1, server2)
  - Generate Flowfile 컴포넌트만 제외하고 나머지 모두 실행 상태로 변경
  - Generate Flowfile 을 껏다가 켰다가 반복하면 플로우파일이 1개씩 생기는데 해당 flowfile이 mysql1 에 입력되고, 
    mysql1 에 입력되면 자동으로 캐치하여 새로 입력된 row만 mysql2 로 카피됨
  
5. FTPtoFTP 설명:
  - ListFTP, FetchFTP, PutFTP 컴포넌트에 password 설정 필요 (dahee)
  - Generate Flowfile, UpdateAttribute 컴포넌트 제외하고 모두 실행 상태로 변경
  - UpdateAttribute 에서 filename 을 수정하고 GenerateFlowfile 을 키면 해당 파일이 ftp에 생성됨
  - 생성된 새로운 파일을 자동으로 감지하여 FetchFTP 가 수행되면 해당 파일의 내용이 LogAttribute 직전 큐에 쌓임
