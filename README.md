# thymeleaf-main-point
타임리프 실무에 사용할 핵심 내용 정리
- 익숙하지 않은 방법들 정리

<h3 style="font-weight:bold">1. Map 타입 data 출력</h3>
JAVA) A, B : 객체 인스턴스 <br>
<span> Map &lt;String, Object&gt; map = new HashMap<>(); </span><br>
map.put("A", A); <br>
map.put("B", B); <br>
model.addAttribute("map", map); <br>
<br>
Template) <br>
&lt;span th:text="${map['A'].data}"&gt;&lt;/span&gt; <br>
------------------------------------------------------------
<h3 style="font-weight:bold;color:red;">2. link (a 태그 href)</h3>
JAVA) <br>
model.addAttribute("param1", data1); <br>
model.addAttribute("param1", data1); <br>
<br>
Template) <br>
&lt;a th:href="@{/hello/{param1}(param1=${param1}, param2=${param2})}"&gt;&lt;/a&gt; <br>
-> /hello/data1?param2=data2 <br>  
------------------------------------------------------------
<h3 style="font-weight:bold">3. javascript 안에서 타임리프 반복</h3>
JAVA) <br>
List&lt;객체&gt; = new ArrayList<>(); <br>
list.add(...)  <br>
... <br>   
model.addAttribute("list", list) <br>
<br>
Template) <br>
&lt;script th:inline="javascript"&gt; <br>
	[# th:each="data, stat : ${list}"] <br>
	var data[[${stat.count}]] = [[${data}]]; <br>
	[/] <br>
&lt;/script&gt; <br>
------------------------------------------------------------ 
<h3 style="font-weight:bold;color:red;">4. switch 문</h3>
Template) <br>  
&lt;td th:switch="${data.status}"&gt; <br>  
    &lt;span th:case="10"&gt;결제 완료&lt;/span&gt; <br>  
    &lt;span th:case="20"&gt;배송 준비중&lt;/span&gt; <br>  
    &lt;span th:case="30"&gt;배송중&lt;/span&gt; <br>  
    &lt;span th:case="*"&gt;기타&lt;/span&gt; <br>  
&lt;/td&gt; <br>   
------------------------------------------------------------ 
<h3 style="font-weight:bold;color:red;">5. 연산으로 null 처리 (true 일때 생략)</h3> 
JAVA) <br>
model.addAttribute("nullData", null); <br>
model.addAttribute("data", "HJ"); <br>  
  <br>
Template) <br>
  &lt;span th:text="${data}?: '데이터가없습니다.'"&gt;&lt;/span&gt; <br>
  &lt;span th:text="${nullData}?: '데이터가 없습니다.'"&gt;&lt;/span&gt; <br>
------------------------------------------------------------
<h3 style="font-weight:bold;color:red;">6. 쿼리파라미터 가져오기 (param 이용)</h3>
URL: ?paramData=data1  <br> 
Template) <br>  
&lt;span th:text="${param.paramData}"&gt;&lt;/span&gt; <br> 
------------------------------------------------------------
<h3 style="font-weight:bold">7. 현재 시간 변경 처리 (localDateTime을 #temporals을 이용해 변경)</h3>
JAVA) <br>  
model.addAttribute("localDateTime", LocalDateTime.now());  <br>
  <br>
Template) <br>
&lt;span th:text="${#temporals.day(localDateTime)}"&gt;&lt;/span&gt; <br>
&lt;span th:text="${#temporals.month(localDateTime)}"&gt;&lt;/span&gt; <br>
&lt;span th:text="${#temporals.year(localDateTime)}"&gt;&lt;/span&gt; <br>
&lt;span th:text="${#temporals.dayOfWeekNameShort(localDateTime)}"&gt;&lt;/span&gt; <br>
&lt;span th:text="${#temporals.hour(localDateTime)}"&gt;&lt;/span&gt; <br>
&lt;span th:text="${#temporals.minute(localDateTime)}"&gt;&lt;/span&gt; <br>
&lt;span th:text="${#temporals.second(localDateTime)}"&gt;&lt;/span&gt; <br>  
------------------------------------------------------------
<h3 style="font-weight:bold;color:red;">8. 템플릿 레이아웃 replace</h3>  
Template) <br>
main.html : <br>
&lt;head th:replace="template/layout/head :: common_header(~{::title},~{::link})"&gt; <br>
 &lt;title>메인 타이틀&lt;/title&gt; <br>
 &lt;link rel="stylesheet" th:href="@{/css/bootstrap.min.css}"&gt; <br>
 &lt;link rel="stylesheet" th:href="@{/themes/smoothness/jquery-ui.css}"&gt; <br>
&lt;/head&gt; <br>
  <br>
head.html : <br>
&lt;head th:fragment="common_header(title,links)"&gt; <br>

 &lt;title th:replace="${title}"&gt;레이아웃 타이틀&lt;/title&gt; <br>
  <br>

 &lt;link rel="stylesheet" type="text/css" media="all" th:href="@{/css/awesomeapp.css}"&gt; <br>
 &lt;link rel="shortcut icon" th:href="@{/images/favicon.ico}"&gt; <br>
 &lt;script type="text/javascript" th:src="@{/sh/scripts/codebase.js}"&gt;&lt;/script&gt; <br>
  <br>

 &lt;th:block th:replace="${links}" /&gt; <br>
&lt;/head&gt;  <br>
