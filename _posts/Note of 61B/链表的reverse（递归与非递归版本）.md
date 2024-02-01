链表的reverse（递归与非递归版本）

```java
1 public void reverse() {
1 IntNode frontOfReversed = null;
2 IntNode nextNodeToAdd = first;
3 while (nextNodeToAdd != null) {
4 IntNode remainderOfOriginal = nextNodeToAdd.next;
5 nextNodeToAdd.next = frontOfReversed;
6 frontOfReversed = nextNodeToAdd;
7 nextNodeToAdd = remainderOfOriginal;
8 }
9 first = frontOfReversed;
```

```java
1 public void reverse() {
2 first = reverseRecursiveHelper(first);
3 }
4
5 private IntNode reverseRecursiveHelper(IntNode front) {
6 if (front == null || front.next == null) {
7 return front;
8 } else {
9 IntNode reversed = reverseRecursiveHelper(front.next);
10 front.next.next = front;
11 front.next = null;
12 return reversed;
13 }
14 }
```





