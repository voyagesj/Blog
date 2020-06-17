출처: https://infotake.tistory.com/88

Cross-Domain으로 인해 외부 데이터를 읽어올 수 없을 경우 이를 해결해 주는 플러그인이다.

AJAX Cross Origin 다운로드 링크
http://www.ajax-cross-origin.com/


코드예제
```JavaScript
<script type="text/javascript" src="자신파일경로/jquery.ajax-cross-origin.min.js"></script>

<script type="text/javascript">

$.ajax({
    crossOrigin : true,
    dataType : "json",
    url : "호출하고자하는주소",
    success : function(data) {
        console.log(data);
    }
});


</script>
```
