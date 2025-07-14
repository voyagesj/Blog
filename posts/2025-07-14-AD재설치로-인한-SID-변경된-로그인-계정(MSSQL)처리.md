## AD재설치로 인한 SID 변경된 로그인 계정(MSSQL)처리

개발 AD를 실수로 복구가 불가능한 상태가 되어 재설치를 진행하였다.

AD 복구 후 이전에 설치한 SQL 서버에 AD 계정으로 접속이 불가능하여 확인해보니 SID 가 달라져서 다른 계정으로 인식하는 문제였다.

이전과 똑같은 계정으로 만들려고 하니 같은 이름의 계정이 있다고 하여 새로 생성은 안되고 기존 계정은 삭제가 안되는 상황.

이유는 다른 데이터베이스의 관리자로 되어있어서였다.

다음과 같이 해결하였다.

```SQL
-- SID 확인
SELECT name, sid, type_desc
FROM sys.server_principals
WHERE name = 'mc\spfarm';

-- 기존 권한 할당된 DB 확인
SELECT name AS DatabaseName, suser_sname(owner_sid) AS Owner
FROM sys.databases
WHERE suser_sname(owner_sid) = 'mc\spfarm';

-- 할당된 권한 sa 로 이양
ALTER AUTHORIZATION ON DATABASE::[Neoplus.Framework.Sample] TO [sa];
ALTER AUTHORIZATION ON DATABASE::[aspnetdb] TO [sa];

-- 계정삭제
drop login [mc\spfarm] 
```
 
