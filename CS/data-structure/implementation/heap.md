# Interface

```java
public interface IHeap<T>{

    void insert(T val);

    boolean contains(T val);

    T pop();

    T peek();

    int size();
}
```

# Implement Heap

```java
package heap;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class MaxHeap<T extends Comparable<T>> implements IHeap<T>{

    private final List<T> list;
    private int size;

    public MaxHeap() {
        list = new ArrayList<>();
        list.add(null);
        this.size = 0;
    }

    public MaxHeap(int maxSize) {
        list = new ArrayList<>(maxSize);
        list.add(null);
        this.size = 0;
    }

    private int getParent(int idx) {
        return idx / 2;
    }

    private int getLeftChild(int idx) {
        return idx * 2;
    }

    private int getRightChild(int idx) {
        return idx * 2 + 1;
    }

    private boolean isLeaf(int pos) {
        if(getLeftChild(pos) >= size)
            return true;

        return (pos > size / 2) && pos <= size;
    }

    private boolean moveUp(int curr) {
        return list.get(getParent(curr)) != null &&
                list.get(curr).compareTo(list.get(getParent(curr))) > 0;
    }

    @Override
    public void insert(T val) {
        list.add(val);
        size++;

        int curr = this.size;

        while (moveUp(curr)) {
            Collections.swap(list, curr, getParent(curr));
            curr = getParent(curr);
        }
    }

    @Override
    public boolean contains(T val) {
        for (T t : list) {
            if(t.equals(val)) {
                return true;
            }
        }
        return false;
    }

    @Override
    public T pop() {
        T top = this.list.get(1);
        this.list.set(1, this.list.get(size));
        this.list.remove(size);
        size--;

        heapify(1);

        return top;
    }

    //heapify는 특정 노드를 중심으로 그 밑의 트리들이 힙 구조를 만족하게끔 만드는 작업
    private void heapify(int idx) {
        if(isLeaf(idx)) return;

        T curr = this.list.get(idx);
        T left = this.list.get(getLeftChild(idx));
        T right = null;

        if(getRightChild(idx) <= size) {
            right = this.list.get(getRightChild(idx));
        }

        //왼쪽 노드만
        if(right == null && curr.compareTo(left) < 0) {
            Collections.swap(list, idx, getLeftChild(idx));
            heapify(getLeftChild(idx));
        } else if (right != null && (curr.compareTo(left) < 0 || curr.compareTo(right) < 0)) { //왼쪽, 오른쪽
            if (left.compareTo(right) > 0) {
                Collections.swap(list, idx, getLeftChild(idx));
                heapify(getLeftChild(idx));
            } else {
                Collections.swap(list, idx, getRightChild(idx));
                heapify(getRightChild(idx));
            }
        }
    }

    @Override
    public T peek() {
        if(this.size == 0)
            throw new NullPointerException();
        return this.list.get(1);
    }

    @Override
    public int size() {
        return size;
    }

    public void printAll() {
        for (int i = 1; i <= size; i++) {
            System.out.print(list.get(i) + " ");
        }
        System.out.println();
    }
}
```
<br>
<br>

# Test Code

```java
public class Main {
    public static void main(String[] args) {

        MaxHeap<Integer> heap = new MaxHeap<>();

        heap.insert(15);
        heap.insert(10);
        heap.insert(8);
        heap.insert(5);
        heap.insert(4);
        heap.insert(20);

        heap.printAll();
        int size = heap.size();
        for (int i = 1; i <= size; i++) {
            System.out.println(heap.pop());
        }
    }
}
```
