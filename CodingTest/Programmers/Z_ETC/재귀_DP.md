# 재귀

- chatgpt

- 팩토리얼<br>
팩토리얼 함수에서 재귀 호출을 사용할 때, 각 호출은 한 단계씩 더 깊은 재귀 단계로 들어갑니다. 이 과정에서 재귀 함수가 종료 조건인 `n <= 1`에 도달하면, 그때부터 각 단계에서 반환된 값이 이전 단계로 올라가면서 최종 결과를 구성합니다. 여기서 `n = 5`의 팩토리얼을 계산하는 과정을 단계별로 설명하겠습니다.

```java
public class Factorial {
    public static int factorial(int n) {
        if (n <= 1) {
            return 1;  // 팩토리얼의 기본 조건, 0! = 1! = 1
        }
        return n * factorial(n - 1);  // 재귀 호출
    }

    public static void main(String[] args) {
        int n = 5;  // 5 팩토리얼을 계산
        System.out.println(n + "! = " + factorial(n));
    }
}
```
### 팩토리얼 함수의 재귀적 계산 과정 (`n = 5`)

1. **첫 번째 호출**: `factorial(5)`
   - 종료 조건에 도달하지 않았으므로 `5 * factorial(4)`를 계산하기 위해 `factorial(4)`를 호출합니다.

2. **두 번째 호출**: `factorial(4)`
   - 마찬가지로 종료 조건에 도달하지 않았으므로 `4 * factorial(3)`을 계산하기 위해 `factorial(3)`을 호출합니다.

3. **세 번째 호출**: `factorial(3)`
   - 여전히 종료 조건에 도달하지 않았으므로 `3 * factorial(2)`를 계산하기 위해 `factorial(2)`를 호출합니다.

4. **네 번째 호출**: `factorial(2)`
   - 여전히 종료 조건에 도달하지 않았으므로 `2 * factorial(1)`를 계산하기 위해 `factorial(1)`를 호출합니다.

5. **다섯 번째 호출**: `factorial(1)`
   - 이번에는 `n <= 1` 조건을 만족하므로 재귀 호출을 종료하고 `1`을 반환합니다.

이제 각 재귀 호출에서 반환된 값이 이전 단계로 올라오는 과정은 다음과 같습니다:

6. **네 번째 반환**: `factorial(2)`에서 `2 * factorial(1)` 계산
   - `factorial(1)`이 `1`을 반환했으므로 `2 * 1 = 2`를 반환합니다.

7. **세 번째 반환**: `factorial(3)`에서 `3 * factorial(2)` 계산
   - `factorial(2)`가 `2`를 반환했으므로 `3 * 2 = 6`를 반환합니다.

8. **두 번째 반환**: `factorial(4)`에서 `4 * factorial(3)` 계산
   - `factorial(3)`이 `6`을 반환했으므로 `4 * 6 = 24`를 반환합니다.

9. **첫 번째 반환**: `factorial(5)`에서 `5 * factorial(4)` 계산
   - `factorial(4)`가 `24`를 반환했으므로 `5 * 24 = 120`를 반환합니다.

최종적으로 `factorial(5)` 호출은 `120`을 반환하며, 이는 `5!`의 값입니다. 이처럼 각 재귀 호출에서 반환된 값은 이전 호출의 결과와 결합되어 최종 결과를 형성하는 중요한 과정입니다.

- 단순 곱 (재귀 예시)
```java
public static int zzzzzzzz(int n){
    if(n <= 1){
        return n;
    }
    return zzzzzzzz(n-1) * 2;
}
```
주어진 함수 `zzzzzzzz`는 입력된 정수 `n`에 대해, `n`이 1 이하일 때 `n`을 반환하고, 그렇지 않은 경우 `n-1`의 함수 결과값에 2를 곱하는 재귀 함수입니다. 이 함수는 각 재귀 단계마다 결과값을 2배로 만듭니다. 여기서 `n = 5`인 경우의 계산 과정을 단계별로 살펴보겠습니다.

### 재귀적 계산 과정 (`n = 5`)

1. **첫 번째 호출**: `zzzzzzzz(5)`
   - `n <= 1`이 아니므로 `zzzzzzzz(5 - 1) * 2` 즉, `zzzzzzzz(4) * 2`를 계산하기 위해 `zzzzzzzz(4)`를 호출합니다.

2. **두 번째 호출**: `zzzzzzzz(4)`
   - `n <= 1`이 아니므로 `zzzzzzzz(4 - 1) * 2` 즉, `zzzzzzzz(3) * 2`를 계산하기 위해 `zzzzzzzz(3)`를 호출합니다.

3. **세 번째 호출**: `zzzzzzzz(3)`
   - `n <= 1`이 아니므로 `zzzzzzzz(3 - 1) * 2` 즉, `zzzzzzzz(2) * 2`를 계산하기 위해 `zzzzzzzz(2)`를 호출합니다.

4. **네 번째 호출**: `zzzzzzzz(2)`
   - `n <= 1`이 아니므로 `zzzzzzzz(2 - 1) * 2` 즉, `zzzzzzzz(1) * 2`를 계산하기 위해 `zzzzzzzz(1)`를 호출합니다.

5. **다섯 번째 호출**: `zzzzzzzz(1)`
   - `n <= 1` 조건을 만족하므로 재귀 호출을 종료하고 `1`을 반환합니다.

이제 각 재귀 호출에서 반환된 값이 이전 단계로 올라오는 과정은 다음과 같습니다:

6. **네 번째 반환**: `zzzzzzzz(2)`에서 `zzzzzzzz(1) * 2` 계산
   - `zzzzzzzz(1)`이 `1`을 반환했으므로 `1 * 2 = 2`를 반환합니다.

7. **세 번째 반환**: `zzzzzzzz(3)`에서 `zzzzzzzz(2) * 2` 계산
   - `zzzzzzzz(2)`가 `2`를 반환했으므로 `2 * 2 = 4`를 반환합니다.

8. **두 번째 반환**: `zzzzzzzz(4)`에서 `zzzzzzzz(3) * 2` 계산
   - `zzzzzzzz(3)`이 `4`을 반환했으므로 `4 * 2 = 8`를 반환합니다.

9. **첫 번째 반환**: `zzzzzzzz(5)`에서 `zzzzzzzz(4) * 2` 계산
   - `zzzzzzzz(4)`가 `8`을 반환했으므로 `8 * 2 = 16`를 반환합니다.

최종적으로 `zzzzzzzz(5)` 호출은 `16`을 반환하며, 이는 각 단계에서 결과가 2배씩 증가한 것을 보여줍니다. 이 함수의 작동 원리는 간단하게 입력된 값에서 시작하여 1이 될 때까지 각 결과를 누적적으로 2배 증가시키는 구조입니다.

- 피보나치
```java
public static int naiveRecursion(int n){
    if(n <= 1){
        return n;
    }
    return naiveRecursion(n-1) + naiveRecursion(n-2);
}
```
주어진 함수 `naiveRecursion`은 재귀적으로 피보나치 수열의 값을 계산하는 함수입니다. 피보나치 수열에서 각 숫자는 그 이전 두 숫자의 합입니다. 재귀 함수는 이전 두 피보나치 숫자의 합을 계산하여 현재 숫자를 반환합니다.

### 재귀적 계산 과정 (`n = 5`)

이제 `n = 5`인 경우의 피보나치 수 계산 과정을 살펴보겠습니다:

1. **첫 번째 호출**: `naiveRecursion(5)`
   - `n <= 1`이 아니므로 `naiveRecursion(5-1) + naiveRecursion(5-2)` 즉, `naiveRecursion(4) + naiveRecursion(3)`를 계산하기 위해 각각의 함수를 호출합니다.

2. **두 번째 호출**: `naiveRecursion(4)`
   - 마찬가지로, `naiveRecursion(4-1) + naiveRecursion(4-2)` 즉, `naiveRecursion(3) + naiveRecursion(2)`를 계산하기 위해 각각의 함수를 호출합니다.

3. **세 번째 호출**: `naiveRecursion(3)`
   - 또한, `naiveRecursion(3-1) + naiveRecursion(3-2)` 즉, `naiveRecursion(2) + naiveRecursion(1)`를 계산하기 위해 각각의 함수를 호출합니다.

이와 같은 과정은 계속해서 재귀적으로 수행되며, `n`이 1 이하가 될 때까지 각 단계에서 `n-1`과 `n-2`의 합을 계산합니다.

### 재귀 호출의 전개

1. `naiveRecursion(5)` 호출은 `naiveRecursion(4) + naiveRecursion(3)`입니다.
2. `naiveRecursion(4)` 호출은 `naiveRecursion(3) + naiveRecursion(2)`입니다.
3. `naiveRecursion(3)` 호출은 `naiveRecursion(2) + naiveRecursion(1)`입니다.
4. `naiveRecursion(2)` 호출은 `naiveRecursion(1) + naiveRecursion(0)`입니다.

여기서 `naiveRecursion(1)`과 `naiveRecursion(0)`는 각각 1과 0을 반환합니다. 이제 이 반환값들을 사용하여 상위 호출로 결과를 전달하며 최종 값을 계산합니다.

### 결과 계산

- `naiveRecursion(2)`는 `1 + 0 = 1`을 반환합니다.
- `naiveRecursion(3)`는 `naiveRecursion(2) + naiveRecursion(1)` 즉, `1 + 1 = 2`을 반환합니다.
- `naiveRecursion(4)`는 `naiveRecursion(3) + naiveRecursion(2)` 즉, `2 + 1 = 3`을 반환합니다.
- `naiveRecursion(5)`는 `naiveRecursion(4) + naiveRecursion(3)` 즉, `3 + 2 = 5`을 반환합니다.

최종적으로 `naiveRecursion(5)` 호출은 5를 반환합니다. 이러한 재귀적 접근은 매우 직관적이지만, 많은 중복 계산을 발생시키기 때문에 효율적이지 않습니다. 각 수치를 여러 번 계산하게 되므로, 이러한 문제는 동적 계획법이나 메모이제이션을 사용하여 효율적으로 해결할 수 있습니다.

```scss
naiveRecursion(5)
├── naiveRecursion(4)
│   ├── naiveRecursion(3)
│   │   ├── naiveRecursion(2)
│   │   │   ├── naiveRecursion(1) = 1
│   │   │   └── naiveRecursion(0) = 0
│   │   └── naiveRecursion(1) = 1
│   └── naiveRecursion(2)
│       ├── naiveRecursion(1) = 1
│       └── naiveRecursion(0) = 0
└── naiveRecursion(3)
    ├── naiveRecursion(2)
    │   ├── naiveRecursion(1) = 1
    │   └── naiveRecursion(0) = 0
    └── naiveRecursion(1) = 1
```

[참조링크1](https://adjh54.tistory.com/201#google_vignette)
```java
바텀업
private int dp(int n) {
   if (n <= 1) {
      return n;
   }
   
   int[] arr = new int[n + 1];
   arr[0] = 0;
   arr[1] = 1;
   
   //0 1 1 2 3 5
   for (int i = 2; i <= n; i++) {
      arr[i] = arr[i - 1] + arr[i - 2];
      System.out.println(Arrays.toString(arr));
   }
   
   return arr[n];
}

탑다운 (재귀에서는 동일한 계산을 하지만 여기는 배열에 저장된 값을 리턴한다.)
int[] dp = new int[100];

public int fib(int n) {
   if (n <= 1) {
      return n;
   }
   if (dp[n] != 0) { // 메모이제이션
      return dp[n];
   }
   dp[n] = fib(n - 1) + fib(n - 2);
   return dp[n];
}

```

[참조링크2](https://hongjw1938.tistory.com/47)
```java
class Fibonacci{
    // DP 를 사용 시 작은 문제의 결과값을 저장하는 배열
    // Top-down, Bottom-up 별개로 생성하였음(큰 의미는 없음)
    static int[] topDown_memo; 
    static int[] bottomup_table;
    public static void main(){
        int n = 5;
        topDown_memo = new int[n+1];
        bottomup_table = new int[n+1];
        
        long startTime = System.currentTimeMillis();
        System.out.println(naiveRecursion(n));
        long endTime = System.currentTimeMillis();
        System.out.println("일반 재귀 소요 시간 : " + (endTime - startTime));
        
        System.out.println();
        
        startTime = System.currentTimeMillis();
        System.out.println(topDown(n));
        endTime = System.currentTimeMillis();
        System.out.println("Top-Down DP 소요 시간 : " + (endTime - startTime));
        
        System.out.println();
        
        startTime = System.currentTimeMillis();
        System.out.println(bottomUp(n));
        endTime = System.currentTimeMillis();
        System.out.println("Bottom-Up DP 소요 시간 : " + (endTime - startTime));
    }
    
    // 단순 재귀를 통해 Fibonacci를 구하는 경우
    // 동일한 계산을 반복하여 비효율적으로 처리가 수행됨
    static int zz = 0;
    public static int naiveRecursion(int n){
        if(n <= 1){
            return n;
        }
        return naiveRecursion(n-1) + naiveRecursion(n-2);
    }
    
    // DP Top-Down을 사용해 Fibonacci를 구하는 경우
    public static int topDown(int n){
        System.out.println("aucqjs:" + n);
        System.out.println("aucqjs:" + Arrays.toString(topDown_memo));
        // 기저 상태 도달 시, 0, 1로 초기화
        if(n < 2) return topDown_memo[n] = n;
        
        // 메모에 계산된 값이 있으면 바로 반환!
        if(topDown_memo[n] > 0) {
            System.out.println("걸림:" + n);
            return topDown_memo[n];
        }
        
        // 재귀를 사용하고 있음!
        topDown_memo[n] = topDown(n-1) + topDown(n-2);
        
        return topDown_memo[n];
    }
    
    // DP Bottom-Up을 사용해 Fibonacci를 구하는 경우
    public static int bottomUp(int n){
        // 기저 상태의 경우 사전에 미리 저장
        bottomup_table[0] = 0; bottomup_table[1] = 1;
        
        // 반복문을 사용하고 있음!
        for(int i=2; i<=n; i++){
            // Table을 채워나감!
            bottomup_table[i] = bottomup_table[i-1] + bottomup_table[i-2];
        }
        return bottomup_table[n];
    }
}
```