```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public String solution(String[] participant, String[] completion) {
        String answer = "";

        Map<String, Integer> map = new HashMap<>();
        for (String key : participant) {
            map.merge(key, 1, Integer::sum);
        }

        for (String s : completion) {
            int value = map.get(s)-1;
            map.put(s, value);
        }

        for (String s : map.keySet()) {
            if(map.get(s) == 1) {
                answer = s;
                break;
            }
        }
        return answer;
    }
}
```
