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

# DoubleLinkedList Code

```java
import java.util.NoSuchElementException;

public class MyDoubleLinkedList<T> implements ILinkedList<T>{

    private Node head = null;
    private Node tail = null;
    private int size = 0;

    private class Node {
        T data;
        Node prev;
        Node next;

        Node(T data) {
            this.data = data;
            prev = null;
            next = null;
        }
    }

    @Override
    public void add(T data) {
        if(head==null) {
            head = new Node(data);
            tail = head;
        } else {
            Node node = new Node(data);
            tail.next = node;
            node.prev = tail;
            tail = tail.next;
        }
        size++;
    }

    @Override
    public void add(int index, T data) {
        if(index < 0 || index > size)
            throw new IndexOutOfBoundsException();

        if(index==0) {
            Node node = new Node(data);
            node.next = head;
            head = node;
        } else if(index == size) {
            add(data);
        } else if (size / 2 > index) {
            Node node = head;
            int i = 0;
            while (i++ < index-1) {
                node = node.next;
            }
            Node newNode = new Node(data);
            newNode.next = node.next;
            node.next = newNode;
            newNode.prev = node;
            newNode.next.prev = newNode;
        } else {
            Node node = tail;
            int i = size;
            while (i-- > index+1) {
                node = node.prev;
            }
            Node newNode = new Node(data);
            newNode.prev = node.prev;
            node.prev = newNode;
            newNode.next = node;
            newNode.prev.next = newNode;
        }
        size++;
    }

    @Override
    public void remove(int index) {
        if(index < 0 || index >= size)
            throw new IndexOutOfBoundsException();

        if(head == null)
            throw new NoSuchElementException();

        if(index == 0) {
            head = head.next;
            head.prev = null;
        } else if(index == size-1) {
            tail = tail.prev;
            tail.next = null;
        } else if(size / 2 > index) {
            Node node = head;
            int i = 0;
            while (i++ < index-1) {
                node = node.next;
            }
            node.next = node.next.next;
            node.next.prev = node;
        } else {
            Node node = tail;
            int i = size-1;
            while (i-- > index+1) {
                node = node.prev;
            }
            node.prev = node.prev.prev;
            node.prev.next = node;
        }
    }

    @Override
    public int size() {
        return size;
    }

    @Override
    public void printAll() {
        Node node = head;
        while (node != null) {
            System.out.print(node.data +" ");
            node = node.next;
        }
        System.out.println();
    }

    public void printAllToTail() {
        Node node = tail;
        while (node != null) {
            System.out.print(node.data +" ");
            node =node.prev;
        }
        System.out.println();
    }
}
```