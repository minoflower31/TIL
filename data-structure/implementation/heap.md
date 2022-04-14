# Implement Heap

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Heap {
    public static List<Integer> heapArr = null;

    public Heap() {
        heapArr = new ArrayList<>();
        heapArr.add(null);
    }

    public void insert(Integer data) {
        heapArr.add(data);

        //최상위 노드만 존재하는 경우
        if (heapArr.size() == 2) return;

        int insertIdx = heapArr.size() - 1;

        while (moveUp(insertIdx)) {
            int parent = insertIdx / 2;

            Collections.swap(heapArr, insertIdx, parent);
            insertIdx = parent;
        }
    }

    public Integer pop() {
        if (heapArr.size() <= 1) return null;

        int ret = heapArr.get(1);
        int popIdx = 1;

        heapArr.set(popIdx, heapArr.get(heapArr.size() - 1));
        heapArr.remove(heapArr.size() - 1);

        while (moveDown(popIdx)) {
            int leftNodeIdx = popIdx * 2;
            int rightNodeIdx = popIdx * 2 + 1;

            //왼쪽 자식 O, 오른쪽 자식 X
            if (rightNodeIdx >= heapArr.size()) {
                Collections.swap(heapArr, popIdx, leftNodeIdx);
                popIdx = leftNodeIdx;
            } else { //왼쪽 자식 O, 오른쪽 자식 O
                if (heapArr.get(leftNodeIdx) > heapArr.get(rightNodeIdx)) {
                    Collections.swap(heapArr, popIdx, leftNodeIdx);
                    popIdx = leftNodeIdx;
                } else {
                    Collections.swap(heapArr, popIdx, rightNodeIdx);
                    popIdx = rightNodeIdx;
                }
            }
        }

        return ret;
    }

    private boolean moveUp(int insertIdx) {
        if (insertIdx <= 1) return false;

        int parent = insertIdx / 2;
        return heapArr.get(insertIdx) > heapArr.get(parent);
    }

    private boolean moveDown(int popIdx) {
        int leftNodeIdx = popIdx * 2;
        int rightNodeIdx = popIdx * 2 + 1;

        if (leftNodeIdx >= heapArr.size())
            return false;

        return heapArr.get(popIdx) < heapArr.get(leftNodeIdx) || heapArr.get(popIdx) < heapArr.get(rightNodeIdx);
    }
}
```
<br>
<br>

# Test Code

```java
public static void main(String[] args) {
        Heap heap = new Heap();

        heap.insert(15);
        heap.insert(10);
        heap.insert(8);
        heap.insert(5);
        heap.insert(4);
        heap.insert(20);

        int size = Heap.heapArr.size();

        for (int i = 1; i < size; i++) {
            System.out.println(heap.pop());
            System.out.println(Heap.heapArr);
        }
    }
```
