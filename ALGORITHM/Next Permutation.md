# Next Permutation 이란?
- 해당 위치에서의 다음 순열을 찾는 방법을 말한다.

## 방법
1. list[i-1] < list[i] 를 만족하는 가장 큰 i를 찾아야 한다.

- 그니까 순열의 마지막 수에서 끝나는 가장 긴 감소 수열(내림차순)을 찾아야 한다. 
    - ex) 9 2 1 7 6 4 3 면 가장 긴 감소 수열은 7 6 4 3 이 된다.

2. j >= i 면서 list[j] > list[i-1] 을 만족하는 가장 큰 j를 찾는다.
- list[i-1] 보다 큰 수 중 가장 작은 수 -> list[i-1] = 1 이니까 list[j] = 3 이다. 

3. list[i-1] 과 list[j] 의 자리를 바꾼다.
- 9 2 3 7 6 4 1

4. list[i] 부터 순열을 뒤집는다.
- 9 2 3 1 4 6 7

# 문제로 풀어보기
[백준 10972](https://www.acmicpc.net/problem/10972)

## 코드
```
public class BOJ_10972_다음순열 {
    public static void main(String[] args) throws Exception{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());
        String [] arr =br.readLine().split(" ");
        int [] list = new int[N];
        for (int i = 0; i < N; i++) {
            list[i] = Integer.parseInt(arr[i]);
        }// 입력부 완
        StringBuilder sb = new StringBuilder();
        if(nextPermutation(list)){
            for(int a: list){
                sb.append(a).append(" ");
            }
        }else {
            sb.append("-1");
        }
        System.out.println(sb);

    }

    public static boolean nextPermutation(int [] list){
        int i = list.length -1;

        // 1. A[i-1] < A[i] 를 만족하는 가장 큰 i를 찾아야 한다.
        while(i > 0 && list[i-1] >= list[i]) {
            i -= 1;
        }

        if(i<=0) return false;

        int j = list.length - 1;

        // 2. j >= i 면서 A[j] > A[i-1] 을 만족하는 가장 큰 j를 찾는다.
        while(list[i-1] >= list[j]) {
            j -= 1;
        }

        // 3. A[i-1] 과 A[j] 의 자리를 바꾼다.
        int temp = list[j];
        list[j] = list[i-1];
        list[i-1] = temp;

        j = list.length-1;


        //4. A[i] 부터 순열을 뒤집는다.
        while(i < j) {
            temp = list[i];
            list[i] = list[j];
            list[j] = temp;
            i += 1;
            j -= 1;
        }

        return true;

    }
}
```