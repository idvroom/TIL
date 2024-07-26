# 재귀(DB)

- 초깃값 1 -> 반복 쿼리 실행 -> 결과에 따라서 계속 재귀
- 0 -> 2 -> 1 -> 2 -> 1 -> 2
- 초깃값, 반복 쿼리의 결과가 여러 개면 계속 반복으로 동작한다.
- 일반적인 언어의 재귀함수와 동일하게 동작한다. 
``` sql
1
with recursive rc as (
  select 1 as n -- 0 초깃값
  union all
  select n + 1 -- 2 반복
    from rc 
   where n < 5 -- 정지 조건
)
select * from rc;

결과 1,2,3,4,5
```
[참고1](https://jjon.tistory.com/entry/Recursive-CTECommon-Table-Expression-%ED%99%9C%EC%9A%A9)
[참고2](https://velog.io/@nayu1105/SQL-%EC%9E%AC%EA%B7%80-%EC%BF%BC%EB%A6%AC)
