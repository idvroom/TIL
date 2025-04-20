## 요청 매핑 및 파라미터 처리 방법

1. 요청 URI 매핑 어노테이션 
```java
//일반
@RequestMapping("/주소", RequestMethod.GET)

// HTTP 메소드별
@PostMapping("/주소")
@GetMapping("/주소")
@PutMapping("/주소")
@DeleteMapping("/주소")
```

2. PathVariable 처리 방식
```java
@GetMapping("/test/{name}/{age}")
//개별
public void test(@PathVariable String name, @PathVariable String age)
//Map 권장하지 않음
public void test(@PathVariable Map<String, String> map)
//VO 방식
public void getVO(@ModelAttribute DataVO dataVO)
```
3. RequestParam 처리 방식
```java
//http://localhost:8099/getRequestParam?name=이름&age=11
@GetMapping("/getRequestParam")
//개별
public void getRequestParam(@RequestParam String name, @RequestParam String age)
//Map
public void getRequestParamMap(@RequestParam Map<String, Object> map) 
//VO 방식
public void getVO(@ModelAttribute DataVO dataVO)
```
