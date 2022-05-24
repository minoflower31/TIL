#Interface
```java
public interface ILinkedList<T> {

    void add(T data);

    void add(int index, T data);

    void remove(int index);

    int size();

    void printAll();
}
```
<br>

# LinkedList Code

```java
import java.util.NoSuchElementException;

public class MyLinkedList<T> implements ILinkedList<T> {

    private Node head = null;
    private int size = 0;

    private class Node {
        T data;
        Node next;

        Node(T data) {
            this.data = data;
            next = null;
        }
    }

    @Override
    public void add(T data) {
        if (head == null) {
            head = new Node(data);
        } else {
            Node node = head;
            while (node.next != null) {
                node = node.next;
            }
            node.next = new Node(data);
        }
        size++;
    }

    @Override
    public void add(int index, T data) {
        if (index < 0 || index > size) {
            throw new IndexOutOfBoundsException();
        }

        if (head == null) {
            head = new Node(data);
            size++;
            return;
        }

        Node newNode = new Node(data);

        if(index == 0) {
            newNode.next = head;
            head = newNode;
        } else {
            Node cur = head;
            int i = 0;
            while (i++ < index-1) {
                cur = cur.next;
            }
            Node nextNode = cur.next;
            cur.next = newNode;
            newNode.next = nextNode;
        }
        size++;
    }

    @Override
    public void remove(int index) {
        if (index < 0 || index >= size) {
            throw new IndexOutOfBoundsException();
        }

        if(head == null) {
            throw new NoSuchElementException();
        }

        if(index==0) {
            head = head.next;
        } else {
            Node node = head;
            int i = 0;
            while (i++ < index-1) {
                node = node.next;
            }
            node.next = node.next.next;
        }
        size--;
    }

    @Override
    public int size() {
        return size;
    }

    @Override
    public void printAll() {
        Node node = head;

        while (node != null) {
            System.out.print(node.data + " ");
            node = node.next;
        }
        System.out.println();
    }
}

```