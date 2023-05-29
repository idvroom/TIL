# Handler Methods

Srping version: 5.3.14

---

### @PathVariable

URI를 통해 파라미터 전달

```java
//변수와 uri 값이 일치하는 경우
@RequestMapping("/test/{id}")
public void getTest(@PathVariable long id) {
}

//일치하지 않는 경우 매칭 되는 값을 지정
@RequestMapping("/test/{number}")
public void getTest(@PathVariable("number") long id) {
}

//map으로 받기
@RequestMapping("/test/{id}/{name}/{age}")
public void getTest(@PathVariable Map<String, String> map) {
  //http://localhost:8081/test/1/이름/10
  //result: {name=이름, id=1, age=10}
  System.out.println(map);
}

//name, value
@RequestMapping("/test/{number}")
public void getTest(@PathVariable(value = "number", name = "number") long id) {
}
```

name, value

- 동일한 기능, 각각 사용 가능, 같이 사용할 경우 값이 동일해야 한다.<br>
  [name vs value](https://stackoverflow.com/questions/42326686/requestparam-name-vs-value-attributes)

required<br>
중요한건 경로를 둘 다 지정해야한다. <br>
링크를 참조한 Case1로는 되지 않아서 case2 방식으로 작성하니 원하는 결과가 나왔다.

```java
//case1
@GetMapping(value = {"/api3/page/", "/api3/page/{id}"})
public String getPageByApi3(@PathVariable(required = false) String id) {
    if (id != null) {
        return id;
    } else {
        return "id is required!";
    }

    /*
    result
    http://localhost:8080/path/api3/page/
    -> id is required!
    http://localhost:8080/path/api3/page/1
    -> 1
    */

    //링크: https://mkyong.com/spring-mvc/spring-pathvariable-annotation/
}

//case2
@RequestMapping(value = {"/test/{id1}/{id2}", "/test/{id1}"})
public void getTest(@PathVariable(required = false) long id1, @PathVariable(required = true) long id2) {
  /*
  result
  http://localhost:8081/test/1
  -> Required URI template variable 'id2' for method parameter type long is not present]
  */
}

```

---

### @RequestParam

쿼리 스트링을 사용한다

```java
@RequestMapping("/test/param")
public void getTestParam(@RequestParam(name = "id") Long id) {
    System.out.println(id);
    //http://localhost:8081/test/param?id=1 -> 1이 출력
}

@RequestMapping(value="/test/param")
public void getTestParam(@RequestParam(value = "id", required = false) Long id) {
}

@RequestMapping(value="/test/param")
public void getTestParam(@RequestParam(value = "id", required = true, defaultValue = "1") Long id) {
}

//파라미터가 많아지는 경우
@RequestMapping(value="/test/param")
public void getTestParam(@RequestParam Map<String, String> map) {
  //http://localhost:8081/test/param?id=1&name=이름
  //result: {id=1, name=이름}
  System.out.println(map);
}

```

name, value는 @PathVariable에서 설명과 동일하다.
<br>
required = true: 값이 없으면 에러 발생하지만 defaultValue를 지정하면 값이 없을 때 defaultValue로 처리된다.
<br>

---

그 외 정규식(PathVariable만 되는듯하다), Optional 사용도 있고 복합적으로도 사용가능하다.

두 기능의 차이
[PathVariable vs RequestParam](https://www.baeldung.com/spring-requestparam-vs-pathvariable)

구글 검색 하여 해외쪽 자료 참고, 국내는 다 비슷하다..<br>
query string vs path variable

---

더 많은 항목 <br>
https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-methods.html
