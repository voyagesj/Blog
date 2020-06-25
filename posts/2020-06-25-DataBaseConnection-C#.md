출처: https://kinanadel.blogspot.com/2018/10/c-mssql-net.html#comment-form

C#에서 데이터베이스에 연결하고, 쿼리를 실행하는 방법 (Mssql, .net) 
 DB는 MSSQL을 기준

```C
string connectionstringName = WebConfigurationManager.ConnectionStrings["TestDB"].ConnectionString;
```

1. 파라미터가 없는 일반 조회 쿼리
```C
//조회 결과 데이터셋
DataSet ds = new DataSet();

//쿼리
string srQuery = "SELECT * FROM TABLE;";

//조회 요청
try
{
   using (SqlConnection conn = new SqlConnection(connectionstringName))
	{
	   SqlDataAdapter adapter = new SqlDataAdapter(srQuery, conn);

	   //조회결과 저장
	   adapter.Fill(ds);
   }
        return ds;
}
catch (Exception ex)
{
   Console.WriteLine("조회 도중 에러 발생 : " + ex.Message);
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

//저장 요청
   using (SqlConnection conn = new SqlConnection(connectionstringName))
	{
	   SqlCommand command = new SqlCommand(srQuery, conn);
	   command.Parameters.AddWithValue("@param1", "변수1");
	   command.Parameters.AddWithValue("@param2", "변수2");

	   //쿼리 실행
	   command.ExecuteNonQuery();
	}
 ```


3. 파라미터가 있는 조회 쿼리

```C
//조회 결과 데이터셋
DataSet ds = new DataSet();

//쿼리 파라미터가 2개 들어가는 SELECT 쿼리
string srQuery = "SELECT * FROM TABLE WHERE COLUMN1 = @param1 AND COLUMN2 = @param2;";

//조회 요청
try
{
      using (SqlConnection conn = new SqlConnection(connectionstringName))
	{
	   SqlDataAdapter adapter = new SqlDataAdapter(srQuery, conn);
	   adapter.SelectCommand.Parameters.Add(new SqlParameter("@param1", "변수1"));
	   adapter.SelectCommand.Parameters.Add(new SqlParameter("@param2", "변수2"));
	   //조회결과 저장
	   adapter.Fill(ds);
   }
}
catch (Exception ex)
{
   Console.WriteLine("조회 중 에러 : " + ex.Message);
}
 ```
 
