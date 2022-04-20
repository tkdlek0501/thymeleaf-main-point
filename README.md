# thymeleaf-main-point
타임리프 실무에 사용할 핵심 내용 정리

<h3 style="font-weight:bold">1. Map data 출력</h3>
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
model.addAttribute("param2", data2); <br>
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
	
------------------------------------------------------------
<h3 style="font-weight:bold;">9. 입력 form 쉽게 처리하는 방법</h3> 	
<h4 style="fond-weight:bold;">9-1. input의 id와 name 타임리프로 대신하기</h4>
<p>
 	th:object = "${item}" 을 쓰고 <br>
	th:field="${item.itemName}" 을 input 에 쓰면 <br>
	이 이름으로 id와 name과 th:value 까지 자동생성 해주고 또한 checkbox에서는 checked 속성까지! <br>
	<br>
	th:field="*{itemName}" : 선택 변수 식 <br>
	으로 줄여서 쓸 수도 있다 (th:object 로 객체 지정시 *로 인식 가능) <br>	 	
</p>
<h4 style="fond-weight:bold;">9-2. checkbox 처리</h4>	
<p>
	체크를 안했을 때는 서버로 값이 아예 안 넘어가서 false 판단을 못하는데,
	그래서 '&lt;input type="hidden" name="_open" value="on" class="form-check-input"&gt;'를 추가해줘야됨 <br>
	checkbox 도 th:field 가 이 hidden 태그 자동 추가 기능을 제공 <br>
</p>
<h4 style="fond-weight:bold;">9-3. each 반복문에서 id - for 처리</h4>	
<p>
	&lt;input th:field="*{field}"&gt; // id는 유니크 해야되므로 타임리프에서 filed1, filed2, ... 이렇게 만들어줌 <br>
	&lt;label th:for="${#ids.prev('input id값')}" &gt; // 이렇게 for 속성 처리하면 동일하게 맞춰짐
</p>
<h4 style="fond-weight:bold;">9-4. selectbox 처리</h4>
<p>
	selectbox 에서도 th:field 쓰면 <br>
	th:each문으로 option 돌릴 때 th:value 와 값이 같으면 자동으로 selected 추가
</p>	
-------------------------------------------------------------
