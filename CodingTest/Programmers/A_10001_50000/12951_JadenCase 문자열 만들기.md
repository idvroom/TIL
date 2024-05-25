# JadenCase 문자열 만들기

연습문제 > JadenCase 문자열 만들기


문제 링크: https://school.programmers.co.kr/learn/courses/30/lessons/12951

-  " " 로 Split 사용하면 뒤에 공백은 처리되지 않아서 문자를 추가하여 처리
- 다른 정답을 보면 미리 소문자로 바꾸고 ""로 잘라서 이전이 공백인지 판단하여 첫 글자를 대문자로 해주었다.

```java
class Solution {
    public String solution(String s) {
        s += "!";
        String[] arr = s.split(" ");
        
        for (int i = 0; i < arr.length; i++) {
            if (arr[i].trim().equals("")) continue;
            arr[i] = arr[i].substring(0, 1).toUpperCase() + arr[i].substring(1, arr[i].length()).toLowerCase();
        }
        
        return String.join(" ", arr).replace("!","");
    }
}
```
