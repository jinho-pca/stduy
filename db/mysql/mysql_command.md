## MySQL  

* ### 계정 생성
```
CREATE USER '{계정 이름}'@'%' IDENTIFIED BY '{비밀번호}';
```
host를 ```'%'```로 설정하면 모든 외부 IP에서 접속할 수 있다.
특정 IP대역에서만 접속하게 설정하려면 ```'IP.%''```로 설정하면 된다.

---

* ### 계정 비밀번호 변경
```
ALTER USER '{계정 이름}'@'%' IDENTIFIED BY '{비밀번호}';
```

---

* ### 계정 권한 부여
```
// 모든 권한 부여
GRANT ALL PRIVILEGES ON {DB 이름}.* TO '{계정이름}'@'%';
// 최종 권한 부여
FLUSH PRIVILEGES;
```
```
// 특정 권한 부여(select, insert, update)
GRANT select, insert, update ON '{DB 이름}.* TO '{계정이름}'@'%';
```