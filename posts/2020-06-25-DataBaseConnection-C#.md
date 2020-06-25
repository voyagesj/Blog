C#에서 데이터베이스에 연결하고, 쿼리를 실행하는 방법 (Mssql, .net) 
 DB는 MSSQL을 기준

1. DB 연동
```
using System.Data.SqlClient;
//DB connection
SqlConnection sqlConnection = null;
//IP
string strDBIP = "127.0.0.1";
//ID
string strDBID = "user";
//PW
string strDBPW = "12345";
//DB
string strDBName = "dbName";
//DB 인스턴스 생성
public SqlConnection connSql()
{
   sqlConnection = new SqlConnection("server =" + strDBIP + "; uid =" + strDBID + "; pwd =" + strDBPW + "; database =" + strDBName);
   //윈도우 인증 계정일 경우엔
   sqlConnection = new SqlConnection("Server= localhost; Database= dbName Integrated Security = True;");
   //DB 오픈
   if (sqlConnection != null && sqlConnection.State == ConnectionState.Closed)
   {
       sqlConnection.Open();
   }
      return sqlConnection;
   }
}

//DB 연동
sqlConnection = connSql();
```


2. 파라미터가 없는 일반 조회 쿼리
```
//조회 결과 데이터셋
DataSet Lds = new DataSet();

//쿼리
string srQuery = "SELECT * FROM TABLE;";

//DB 연결
SqlConnection sqlConnection = connSql();

//조회 요청
try
{
   SqlConnection sqlConnection = connSql();
   SqlDataAdapter Ldap = new SqlDataAdapter(srQuery, sqlConnection);

   //조회결과 저장
   Ldap.Fill(Lds);
}
catch (Exception exe)
{
   Console.WriteLine("조회 도중 에러 발생 : " + exe.Message);
}
```
 

조회 결과를 사용할땐
```
Lds.Tables[0].Rows[i][j].ToString().Trim();
 ```
 
2. 추가, 삭제 쿼리 실행하는 방법
```
//쿼리 파라미터가 2개 들어가는 INSERT 쿼리
string srQuery = "INSERT INTO TABLE (COLSUMN1 ,COLUMN2) VALUES (@p1 ,@p2);";
//DB 연결
SqlConnection sqlConnection = connSql();
//저장 요청
   SqlConnection sqlConnection = connSql();
   SqlCommand commS = new SqlCommand(srQuery, sqlConnection);
   commS.Parameters.AddWithValue("@p1", "변수1");
   commS.Parameters.AddWithValue("@p2", "변수2");

   //쿼리 실행
   commS.ExecuteNonQuery();

 ```


3. 파라미터가 있는 조회 쿼리

``` 
//조회 결과 데이터셋
DataSet Lds = new DataSet();

//쿼리 파라미터가 2개 들어가는 SELECT 쿼리
string srQuery = "SELECT * FROM TABLE WHERE COLUMN1 = @P1 AND COLUMN2 = @P2;";

//DB 연결
SqlConnection sqlConnection = connSql();

//조회 요청
try
{
   SqlConnection sqlConnection = connSql();
   SqlDataAdapter Ldap = new SqlDataAdapter(srQuery, sqlConnection);
   Ldap.SelectCommand.Parameters.Add(new SqlParameter("@P1", "변수1"));
   Ldap.SelectCommand.Parameters.Add(new SqlParameter("@P2", "변수2"));
   //조회결과 저장
   Ldap.Fill(Lds);
}
catch (Exception exe)
{
   Console.WriteLine("조회 중 에러 발생 : " + exe.Message);
}
 ```
 
