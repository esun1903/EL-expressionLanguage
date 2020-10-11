### EL (Expression Language)

* EL은 표현을 위한 언어로 JSP 스크립트의 표현식을 대신하여 속성 값을 쉽게 출력하도록 고안된 language 이다 
* 즉 표현식 (<%=%>)를 대체할 수 있다. 
* EL 표현식에서 도트 연산자 왼쪽은 반드시 java.util.Map 객체 또는 Java Bean 객체여야 한다. 
* EL 표현식에서 도트 연산자 오른쪽은 반드시 맵의 키이거나 Bean 프로퍼티여야 한다. 

### EL에서 제공하는 기능 

* JSP의 네 가지 기본 객체가 제공하는 영역의 속성 사용 

* 자바 클래스 메소드 호출 기능 

* 표현 언어만의 기본 객체 제공

* 수치, 관계, 논리 연산 제공

###  EL 문법 

* ${userinfo.zipDto.address}
* map 을 사용하는 경우 -> ${map     .   map의 키 }
* Java Bean을 사용하는 경우 -> ${JavaBean     .    Bean 프로퍼티}

### EL 문법 : [] 연산자 

* EL에는 Dot 표기법 외에 [] 연산자를 사용하여 객체의 값에 접근할 수 있다. 
* [] 연산자 안의 값이 문자열인 경우 , 이것은 맵의 키가 될 수도 있고 , Bean 프로퍼티나 리스트 및 배열의 인덱스가 될 수 있다. 
* 배열과 리스트인 경우, 문자로 된 인덱스 값은 숫자로 변경하여 처리합니다. 

```jsp
//Servlet 
String[] names = {"홍길동" , "이순신" , "임꺽정"};
request.setAttribute("userNames" , names);

//JSP
${userNames[0]};  //홍길동 출력
${userNames["1"]};  // 문자열인 인덱스 값이 숫자로 바뀌어 userNames[1]의 결과인 이순신 출력.
```

### EL 내장객체 : EL 내장객체는 JSP 페이지의  EL표현식에서 사용할 수 있는 객체 

| category        | identifier       | type      | description                                                  |
| --------------- | ---------------- | --------- | ------------------------------------------------------------ |
| JSP             | pageContext      | Java Bean | 현재 페이지의 프로세싱과 상응하는 PageContext instance       |
| 범위            | pageScope        | Map       | page scope에 저장된 객체를 추출                              |
| 범위            | **requestScope** | Map       | request scope에 저장된 객체를 추출                           |
| 범위            | **sessionScope** | Map       | session scope에 저장된 객체를 추출                           |
| 범위            | applicationScope | Map       | application scope에 저장된 객체를 추출                       |
| 요청 매개변수   | param            | Map       | ServletRequest.getParameter(String)을 통해 요청 정보를 추출  |
| 요청 매개변수   | paramValues      | Map       | ServletRequest.getParameterValues(String)을 통해 요청 정보를 추출 |
| 요청 헤더       | header           | Map       | HttpServletRequest.getHeader(String)을 통해 헤더 정보를 추출 |
| 요청 헤더       | headerValues     | Map       | HttpServletRequest.getHeaderValues(String)을 통해 헤더 정보를 추출 |
| 쿠키            | cookie           | Map       | HttpServletRequest.getCookie()를 통해 쿠키 정보를 추출       |
| 초기화 매개변수 | initParam        | Map       | ServletContext.getInitParameter(String)를 통해 초기화 파라미터를 추출 |

### EL 사용

* pageContext를 제외한 모든 EL 내장 객체는 Map이다 
* 그러므로 key와 value의 쌍으로 값을 저장하고 있다. 
* 기본 문법  : ${expr}

### EL에서 객체 접근 

* request.setAttribute("userinfo","안효인");
* 1. ${requstScope.userinfo} 
* 2. ${pageContext.request.userinfo}, ${userinfo}
* url?name = 안효인&fruit=사과&fruit=바나나
* 1. ${param.name}
* 2. ${paramValues.fruit[0]} , ${paramValues.fruit[1]};

* request.setAttribute("ssafy.user",memberDto);
* ${safy.user.name} //싸피라는 속성은 존재하지 않음
* ${requestScope["ssafy.user"].name}  //request 내장객체에서 [] 연산자를 통해 속성 접근 
* ${cooke.id.value}
* 1. Cookie가 null이라면 null return 
* 2. null이 아니라면 id를 검사 후 null 이라면 null return 
* 3. null이 아니라면 value값 검사 
* -> EL은 값이 null이라도 null을 출력하지 않는다.(공백출력)

### EL Operation(연산자)

* 대부분 java와 동일 

| description |                    |
| ----------- | ------------------ |
| 산술        | +, - , * , / , %   |
| 관계형      | ==,!=,<,>,<=,>=    |
| 3항 연산    | 조건 ? 값 1 : 값 2 |
| 논리        | && , \|\|  , !     |
| 타당성 검사 | empty              |

* empty 연산자에서 true를 return 하는 경우 -> ${empty var}
  * 값이 null이면 true 
  * 값이 빈 문자열("") 이면 true
  * 길이가 0인 배열([]) 이면 true 
  * 빈 Map 객체는 true 
  * 빈 Collection 객체는 true 

### EL에서 객체 method 호출 

```jsp
<% 
  List<MemberDto> list = dao.getMembers();
  request.setAttribute("user",list);
%>
```

* 회원 수 : ${requestScope.users.size()} , ${users.size()}

* 주의 : ${users.size} == <%=request.getAttribute("users").getSize() %>

  
