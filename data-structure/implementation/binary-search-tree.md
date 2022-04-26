# Interface
```java
public interface ITree<T> {

    void insert(T data);

    void delete(T data);

    boolean contains(T data);

    int size();
}
```

# BST Code

```java
public class MyBinarySearchTree<T extends Comparable<T>> implements ITree<T> {

    public Node root;
    private int size;

    public MyBinarySearchTree() {
        root = null;
        size = 0;
    }

    @Override
    public void insert(T data) {
        if(root == null) {
            root = new Node(data);
            size++;
            return;
        }

        Node curr = root;
        while (true) {
            if(data.compareTo(curr.data) < 0) {
                if(curr.left != null)
                    curr = curr.left;
                else {
                    curr.left = new Node(data);
                    size++;
                    break;
                }
            } else {
                if(curr.right != null)
                    curr = curr.right;
                else {
                    curr.right = new Node(data);
                    size++;
                    break;
                }
            }
        }
    }

    @Override
    public void delete(T data) {
        if(root == null) return;
        if(root.data == data && root.left == null && root.right == null) {
            root = null;
            size--;
            return;
        }

        Node currParent = root;
        Node curr = root;
        boolean flag = false;

        while (curr != null) {
            if(curr.data == data) {
                flag = true;
                break;
            }
            currParent = curr;

            if(data.compareTo(curr.data) < 0) {
                curr = curr.left;
            } else {
                curr = curr.right;
            }
        }
        if(!flag) return;

        if(curr.left == null && curr.right == null) { //제거할 node가 leaf node
            if(curr.data.compareTo(currParent.data) < 0) {
                currParent.left = null;
            } else {
                currParent.right = null;
            }
        } else if(curr.left != null && curr.right == null) { // 제거할 node가 하나의 자식을 갖는 경우
            if(curr.data.compareTo(currParent.data) < 0) { //왼쪽 자식이 존재
                currParent.left = curr.left;
            } else { //오른쪽 자식이 존재
                currParent.right = curr.left;
            }
        } else if(curr.left == null && curr.right != null) {
            if(curr.data.compareTo(currParent.data) < 0) {
                currParent.left = curr.right;
            } else {
                currParent.right = curr.right;
            }
        } else { //제거할 node가 자식을 모두 가진 경우
            Node change = curr.right;
            Node changeParent = curr.right;

            while (change.left != null) {
                changeParent = change;
                change = change.left;
            }

            if(change.right != null) {
                changeParent.left = change.right;
            } else {
                changeParent.left = null;
            }
            
            if(curr.data.compareTo(currParent.data) < 0) {
                currParent.left = change;
            } else {
                currParent.right = change;
            }
            change.left = curr.left;
            change.right = curr.right;
        }
        size--;
    }

    @Override
    public boolean contains(T data) {
        return containsRec(data, this.root);
    }

    private boolean containsRec(T data, Node node) {
        if(node == null) return false;
        if(data.compareTo(node.data) == 0) return true;

        if(data.compareTo(node.data) < 0) {
            return containsRec(data, node.left);
        }

        return containsRec(data, node.right);
    }

    @Override
    public int size() {
        return this.size;
    }

    public class Node{
        T data;
        Node left;
        Node right;

        Node(T data) {
            this.data = data;
            this.left = null;
            this.right = null;
        }

        public T getData() {
            return data;
        }

        public Node getLeft() {
            return left;
        }

        public Node getRight() {
            return right;
        }
    }
}
```