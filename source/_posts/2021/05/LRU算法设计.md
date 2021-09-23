---
title: LRU算法设计
date: 2021-05-07 11:08:04
tags:
 - leetcode
 - LRU
categories:
 - leetcode
math:
---

# LRU缓存算法设计

## 机制简单介绍

每次放入缓存中的数据会到队列头，如果满了会将队列尾部的删除，每次访问的也放到队列头，也就是根据访问的时序来进行淘汰。

<!-- more -->

## 代码设计

`LRUCache`数据结构需要满足以下几点：

1. 其中的元素必须有时序，用来区分最近使用和久未使用的数据，容量满了之后要删除最久未使用的那个元素来腾位置。
2. 能够迅速地判断`key`是否存在，并且得到对应的`val`
3. 每次访问某个`key`时，需要将这个元素变为最近使用的，也就是该数据结构需要支持在任意位置快速插入和删除元素

满足以上条件的数据结构为`LinkedHashMap`，根据该结构对以上的条件进行分析：

1. 如果默认从链表尾部添加元素，那么显然越靠近尾部的元素就越是最近使用地，越靠近头部的元素就是越久未使用的。
2. 对于某个`key`，可以通过哈希表快速定位到链表中的节点，从未取得对应的`val`
3. 链表支持在任意位置快速插入和删除

## 代码实现

### 自己实现双向链表

自定义双向链表：

```java
public class DoubleList {
    private Node head, tail;
    private int size;

    public DoubleList(){
        head = new Node(0, 0);
        tail = new Node(0, 0);
        head.next = tail;
        tail.next = head;
        size = 0;
    }

    public void addLast(Node x){
        x.prev = tail.prev;
        x.next = tail;
        tail.prev.next = x;
        tail.prev = x;
        size++;
    }

    public void remove(Node x){
        x.prev.next = x.next;
        x.next.prev = x.prev;
        size--;
    }

    public Node removeFirst(){
        if(head.next == tail){
            return null;
        }
        Node first = head.next;
        remove(first);
        return first;
    }

    public int size() { return size; };
}

class Node{
    public int key, value;
    public Node next, prev;
    public Node(int k, int v){
        key = k;
        value = v;
    }
}
```

自定义LRU缓存数据结构：

```java
import java.util.HashMap;
import java.util.Map;

public class LRUCache {
    private final Map<Integer, Node> map;
    private final DoubleList cache;

    private final int cap;
    public LRUCache(int capacity){
        this.cap = capacity;
        map = new HashMap<>();
        cache = new DoubleList();
    }

    //将某个Key设置为最近使用
    private void makeRecently(int key){
        Node node = map.get(key);

        cache.remove(node);
        cache.addLast(node);
    }

    //添加最近使用的元素
    private void addRecently(int key, int val){
        Node node = new Node(key, val);
        cache.addLast(node);
        map.put(key, node);
    }

    //删除一个Key
    private void deleteKey(int key){
        Node node = map.get(key);
        cache.remove(node);
        map.remove(key);
    }

    //删除最久未使用的元素
    private void deleteLeastRecently(){
        Node deletedNode = cache.removeFirst();
        int deletedKey = deletedNode.key;
        map.remove(deletedKey);
    }

    public int get(int key){
        if(!map.containsKey(key)){
            return -1;
        }
        //将元素提升为最近使用的
        makeRecently(key);
        return map.get(key).value;
    }

    public void put(int key, int value){
        if(map.containsKey(key)){
            deleteKey(key);
            addRecently(key, value);
            return;
        }
        if(cap == cache.size()){
            deleteLeastRecently();
        }
        addRecently(key, value);
    }
}

```

### 使用自带的哈希链表

```java
import java.util.LinkedHashMap;
import java.util.Map;

public class LRUCache {
    int cap;
    Map<Integer, Integer> cache = new LinkedHashMap<>();

    public LRUCache(int capacity){
        cap = capacity;
    }

    public int get(int key){
        if(!cache.containsKey(key)){
            return -1;
        }
        //将key变为最近使用
        makeRecently(key);
        return cache.get(key);
    }

    public void put(int key, int value){
        if(cache.containsKey(key)){
            //修改key的值
            cache.put(key, value);
            //将key变为最近使用
            makeRecently(key);
            return;
        }

        if(cache.size() > cap){
            int oldestKey = cache.keySet().iterator().next();
            cache.remove(oldestKey);
        }
        cache.put(key, value);
    }

    public void makeRecently(int key){
        int val = cache.get(key);
        cache.remove(key);
        cache.put(key, val);
    }
}
```

