# Handler Methods

Srping version: 5.3.14

---

### @ModelAttribute

```java
@Data
public class ParamVO {
    private long id;
    private String name;
}

@RequestMapping(value="/test/model")
public void getTestModel(@ModelAttribute ParamVO vo) {
    //http://localhost:8081/test/model?name=이름&id=1
    //http://localhost:8081/test/model?id=1&name=이름
    //result: ParamVO(id=1, name=이름)
}

@RequestMapping(value="/test/{id}/model/{name}")
public void getTestModel(@ModelAttribute ParamVO vo) {
    //http://localhost:8081/test/1/model/이름
}
```

name, value는 동일한 기능이고 jsp 페이지에서 사용할 이름이다.

```java
@RequestMapping(value="/test/model")
public String getTestModel(@ModelAttribute(name = "p") ParamVO vo) {
    return "model";
}

@RequestMapping(value="/test/model")
public String getTestModel(@ModelAttribute("p") ParamVO vo) {
    return "model";
}

@RequestMapping(value="/test/model")
public String getTestModel(@ModelAttribute ParamVO vo) {
    //지정하지 않을 경우 paramVO.id, paramVO.name
    return "model";
}

```

```javascript
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>User Details</title>
</head>
<body>
   <p><strong>User ID:</strong> ${p.id}</p>
   <p><strong>Name:</strong> ${p.name}</p>
</body>
</html>
```

binding = false는 기존에 가지고 있는 값을 수정하지 않는 용도로 사용 되는 듯 하다.. 잘 쓰진 않는 것으로 보인다.<br>
[binding option](https://stackoverflow.com/questions/27744890/spring-mvc-how-to-forbid-data-binding-to-modelattribute)

---

BindingResult는 binding error를 처리해준다. 숫자 타입에 문자를 넣었을 때 동작하는 방식이다.

```java
//호출 http://localhost:8081/test/model?id=zz&name=이름

@RequestMapping(value="/test/model")
public void getTestModel(@ModelAttribute ParamVO vo) {
    //BindException 발생
}

@RequestMapping(value="/test/model")
public void getTestModel(@ModelAttribute ParamVO vo, BindingResult result) {
    System.out.println(result.hasErrors()); //true
    System.out.println(vo); //ParamVO(id=0, name=이름)
}
```

아래 링크 글 참고하기에 좋다<br>

[@ModelAttribute 장점](https://galid1.tistory.com/769)

---

@Vaild

라이브러리 추가: spring-boot-starter-validation

다양한 옵션, 범위를 지정할 수 있고 Size에서 한글은 1글자로 인식한다.<br>
동일하게 BindingResult로 바인딩 값 에러를 따로 처리가 가능하다.

[참고 및 읽어볼만한](https://velog.io/@minji/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-Spring-Boot%EC%9D%98-Validation-Valid-%EC%96%B4%EB%85%B8%ED%85%8C%EC%9D%B4%EC%85%98%EC%9C%BC%EB%A1%9C-%EC%9C%A0%ED%9A%A8%EC%84%B1-%EA%B2%80%EC%A6%9D%ED%95%98%EA%B8%B0)

```java
@Data
public class ParamVO {
    @Min(10)
    private long id;
    @Size(min = 1, max=10, message = "이름은 최소 1글자부터 10글자여야 합니다.")
    private String name;
}

@RequestMapping(value="/test/model")
public void getTestModel(@ModelAttribute @Valid ParamVO vo) {
    //http://localhost:8081/test/model?id=5&name=이름 -> 10 이상이어야 합니다
    //http://localhost:8081/test/model?id=11&이름입니다이름입니다1 -> 이름은 최소 1글자부터 10글자여야 합니다.
}

@RequestMapping(value="/test/model")
public void getTestModel(@ModelAttribute @Valid ParamVO vo, BindingResult result) {
  //result.hasErrors() 로 처리
}

//단일 파라미터 처리
//class 상단에 @Validated
//https://www.baeldung.com/spring-validate-requestparam-pathvariable
@RequestMapping(value="/test/valid")
public void getValid(@RequestParam @Min(3) int id) {
    //http://localhost:8081/test/valid?id=2 -> getValid.id: 3 이상이어야 합니다
}

```

---

@Validated<br>

[참고 자료이고 설명 잘 되어 있다](https://ojt90902.tistory.com/673)<br>

group 사용법<br>
size는 저장, 수정 시에 필수값이다.<br>
size는 저장 시에만 10이하로 저장해야하고 수정 시는 자유롭게 지정 가능하다.<br>

```java

//그룹 분리용 인터페이스
public interface Insert {
}
public interface Update {
}

@Data
public class ParamVO {
    @NotNull(groups = {Insert.class, Update.class})
    @Max(value = 10, groups = Insert.class)
    private int size;
}

@RequestMapping(value="/test/model/insert")
public void validatedInsert(@Validated(Insert.class) @ModelAttribute ParamVO vo, BindingResult result) {
    //http://localhost:8081/test/model/insert?size=100
    //->10 이하여야 합니다
    if(result.hasErrors()) {
        System.out.println(result.getFieldError());
    }
    //System.out.println(vo);
}

@RequestMapping(value="/test/model/update")
public void validatedUpdate(@Validated(Update.class) @ModelAttribute ParamVO vo, BindingResult result) {
    //http://localhost:8081/test/model/update?size=100
    //-> 정상적으로 동작
    if(result.hasErrors()) {
        System.out.println(result.getFieldError());
    }
    //System.out.println(vo);
}

```
