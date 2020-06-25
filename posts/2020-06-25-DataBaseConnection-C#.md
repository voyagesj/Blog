출처: https://kinanadel.blogspot.com/2018/10/c-mssql-net.html#comment-form
C#에서 데이터베이스에 연결하고, 쿼리를 실행하는 방법 (Mssql, .net) 
 DB는 MSSQL을 기준

1. DB 연동
```C
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
```C
//조회 결과 데이터셋
DataSet ds = new DataSet();

//쿼리
string srQuery = "SELECT * FROM TABLE;";

//DB 연결
SqlConnection sqlConnection = connSql();

//조회 요청
try
{
   SqlConnection sqlConnection = connSql();
   SqlDataAdapter adapter = new SqlDataAdapter(srQuery, sqlConnection);

   //조회결과 저장
   adapter.Fill(ds);
}
catch (Exception exe)
{
   Console.WriteLine("조회 도중 에러 발생 : " + exe.Message);
}
```
 

조회 결과를 사용할땐
```C
ds.Tables[0].Rows[i][j].ToString().Trim();
 ```
 
2. 추가, 삭제 쿼리 실행하는 방법
```C
//쿼리 파라미터가 2개 들어가는 INSERT 쿼리
string srQuery = "INSERT INTO TABLE (COLSUMN1 ,COLUMN2) VALUES (@param1 ,@param2);";
//DB 연결
SqlConnection sqlConnection = connSql();
//저장 요청
   SqlConnection sqlConnection = connSql();
   SqlCommand commS = new SqlCommand(srQuery, sqlConnection);
   commS.Parameters.AddWithValue("@param1", "변수1");
   commS.Parameters.AddWithValue("@param2", "변수2");

   //쿼리 실행
   commS.ExecuteNonQuery();

 ```


3. 파라미터가 있는 조회 쿼리

```C
//조회 결과 데이터셋
DataSet ds = new DataSet();

//쿼리 파라미터가 2개 들어가는 SELECT 쿼리
string srQuery = "SELECT * FROM TABLE WHERE COLUMN1 = @param1 AND COLUMN2 = @param2;";

//DB 연결
SqlConnection sqlConnection = connSql();

//조회 요청
try
{
   SqlConnection sqlConnection = connSql();
   SqlDataAdapter adapter = new SqlDataAdapter(srQuery, sqlConnection);
   adapter.SelectCommand.Parameters.Add(new SqlParameter("@param1", "변수1"));
   adapter.SelectCommand.Parameters.Add(new SqlParameter("@param2", "변수2"));
   //조회결과 저장
   adapter.Fill(ds);
}
catch (Exception exe)
{
   Console.WriteLine("조회 중 에러 발생 : " + exe.Message);
}
 ```
 
