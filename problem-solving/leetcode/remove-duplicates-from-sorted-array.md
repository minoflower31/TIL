# remove-duplicates-from-sorted-array

출처: https://leetcode.com/problems/remove-duplicates-from-sorted-array/

<br>

### Runtime
<img width="632" alt="image" src="https://github.com/minoflower31/TIL/assets/56334513/2c3fb53d-a679-4325-a00f-2f394002425d">

### Memory
<img width="629" alt="image" src="https://github.com/minoflower31/TIL/assets/56334513/55377404-2f2c-4f90-b3b6-90394da39e0c">

<br>
<br>

## 접근 방법

- boolean[] 배열로 중복을 제거하려 했다.
- `nums` 배열 원소의 크기는 -100 ~ 100까지이기 때문에 각각 100을 더하면 0 ~ 200까지의 배열을 선언할 수 있기에 크기가 201인 boolean 배열을 초기화했다.
- 0부터 nums의 크기만큼 반복문을 돌리되, 특정 원소가 false라면 한 번도 중복되지 않은 값이라 생각하여 해당 조건을 통해 중복을 판별했다.

<br>
<br>

## 풀이 코드

```java
class Solution {

    private static final int MAX_SIZE = 200;

    public int removeDuplicates(int[] nums) {
        boolean[] check = new boolean[MAX_SIZE + 1];
        int k = 0;
        for(int i = 0; i < nums.length; i++) {
            if(!check[nums[i] + 100]) {
                nums[k] = nums[i];
                check[nums[i] + 100] = true;
                k++;
            }
        }
        return k;
    }
}
```

<br>
<br>

## 1위를 차지한 코드

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int j = 0;
        for(int i =0; i<nums.length; i++){
            if(nums[j] != nums[i]){
                nums[++j] =nums[i];
            }
        }
        return ++j;
    }
}
```

- j 변수를 기준으로 하나씩 nums의 값을 채워가는 게 인상깊었다.
- 만약 연습장에서 도식화하여 문제를 풀었다면 비슷한 풀이가 나왔을텐데 아쉽다.
  - 아직 감을 잡는 단계이기 때문에 충분히 연습장에서 결과를 어느 정도 도출한 후에 문제를 풀어야겠다.
