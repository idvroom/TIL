# @RequestMapping

요청이 들어오는 uri 매핑 처리 하기 위한 어노테이션

```java
@RequestMapping(value="/test", method = RequestMethod.GET)
public void getTest() throws Exception{
}

//사용한 가능한 RequestMethod
public enum RequestMethod {
	GET, HEAD, POST, PUT, PATCH, DELETE, OPTIONS, TRACE
}
```

이런 방식으로도 사용이 가능하다.

```java
//4.3 vesion
@GetMapping("/test")
@PostMapping("/test")
@PutMapping("/test")
@DeleteMapping("/test")
@PatchMapping("/test")

//여러 요청
@RequestMapping(
  value = "/test",
  method = { RequestMethod.GET, RequestMethod.POST }
)

//method를 지정하지 않을 경우 아래의 HTTP의 모든 요청을 받는다.
//GET, POST, HEAD, OPTIONS, PUT, PATCH, DELETE, TRACE
@RequestMapping(value="/test")

```

[method 기본값](https://stackoverflow.com/questions/38811606/what-is-the-default-request-method-type-for-the-request-mapping)

PUT, POST 차이<br>
[각 요청에 대한 참고 링크1](https://javaplant.tistory.com/18)<br>
[각 요청에 대한 참고 링크2](https://sun-22.tistory.com/41)

찾아보면 GET, POST 외는 취약점이 있다고 하지만 의견들이 다 다르다.<br>
[HTTP 요청 취약점](https://mokpo.tistory.com/202)

value 옵션

```java
//단독 사용
@RequestMapping("/test")

//다중 사용
@RequestMapping({"/test1", "/test2"})

//다른 옵션이 있는 경우(value 생략 불가)
@RequestMapping(value="/test", method = RequestMethod.GET)

//{} 사용할 경우 @PathVariable로 인자를 받아야 함
// @RequestMapping("/test/{id}")
@RequestMapping("/test/{id}/{value}") //다중 @PathVariable
public void getTest(@PathVariable long id, @PathVariable String value) {
}

//?, *, **
@RequestMapping("pattern??")
public void pattern1(){
    System.out.println("pattern1");
    //http://localhost:8081/patternaa
    //http://localhost:8081/patternbb
}

@RequestMapping("pattern/*")
public void pattern2(){
    System.out.println("pattern2");
    //http://localhost:8081/pattern/a
    //http://localhost:8081/pattern/b
}

@RequestMapping("pattern/**")
public void pattern3(){
    System.out.println("pattern3");
    //http://localhost:8081/pattern/a/b
    //http://localhost:8081/pattern/a/c
    //http://localhost:8081/pattern/a/b/c
}

//정규식 방식은 다른 링크 참고

```

@PathVariable 간단한 사용법<br>
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
```

비슷하게 사용하는 @RequestParam 간단한 사용법<br>
쿼리 스트링 사용

```java
@RequestMapping("/test/param")
public void getTestParam(@RequestParam(name = "id") Long id) {
    System.out.println(id);
    //http://localhost:8081/test/param?id=1 -> 1이 출력
}
```

<br>

잘 사용되지 않는 것으로 보여서 간단히<br>
params: 쿼리스트링 유무 등 지정<br>
consumes: 요청 타입 지정<br>
produce: 응답 타입 지정

[참고](https://hooongs.tistory.com/54)<br>

```java
//다른 방식들은 검색.
//쿼리 스트링이 id로만 들어와야 함, multi도 가능
//http://localhost:8081/test/param?id=1
//http://localhost:8081/test/param?id2=1 <- error
@RequestMapping(value="/test/param", params = "id")
public void getTestParam(@RequestParam(name = "id") Long id) {}

@GetMapping(value = "/{rno}",
produces = {MediaType.APPLICATION_XML_VALUE,
            MediaType.APPLICATION_JSON_UTF8_VALUE})

@PostMapping(value = "/new",
	        consumes = "application/json",
	        produces = {MediaType.TEXT_PLAIN_VALUE})
```

[읽어볼만한 링크1](https://www.baeldung.com/spring-requestmapping)<br>
[읽어볼만한 링크2](https://tecoble.techcourse.co.kr/post/2021-06-18-spring-request-mapping/)<br>
