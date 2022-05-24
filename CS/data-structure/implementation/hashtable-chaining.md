# Interface

```java
public interface IHashTable<K,V> {
    int DEFAULT_SIZE = 1024;

    void put(K key, V value);

    V get(K key);

    boolean delete(K key);

    boolean contains(K key);

    int size();
}
```

# Hashtable Chaining Code

```java
import java.util.LinkedList;

public class HashTableChaining<K,V> implements IHashTable<K,V>{

    private LinkedList<Node>[] hashTable;
    private int size;

    public HashTableChaining() {
        this.hashTable = new LinkedList[DEFAULT_SIZE];
        this.size = 0;

        for (int i = 0; i < DEFAULT_SIZE; i++) {
            hashTable[i] = new LinkedList<>();
        }
    }

    @Override
    public void put(K key, V value) {
        int address = hashFunction(key);
        hashTable[address].add(new Node(key,value));
        size++;
    }

    @Override
    public V get(K key) {
        int address = hashFunction(key);

        for (Node node : hashTable[address]) {
            if(node.key == key) {
                return node.value;
            }
        }
        return null;
    }

    @Override
    public boolean delete(K key) {
        int address = hashFunction(key);

        for(Node node: hashTable[address]) {
            if(node.key == key) {
                hashTable[address].remove(node);
                size--;
                return true;
            }
        }
        return false;
    }

    @Override
    public boolean contains(K key) {
        int address = hashFunction(key);

        for(Node node: hashTable[address]) {
            if(node.key == key) {
                return true;
            }
        }
        return false;
    }

    @Override
    public int size() {
        return size;
    }

    private class Node {
        K key;
        V value;

        public Node(K key, V value) {
            this.key = key;
            this.value = value;
        }
    }

    private int hashFunction(K key) {
        String s = key.toString();
        int sum = 0;
        for (int i = 0; i < s.length(); i++) {
            sum += s.charAt(i);
        }
        return sum % this.DEFAULT_SIZE;
    }
}
```