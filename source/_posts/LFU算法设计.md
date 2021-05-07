---
title: LFU算法设计
date: 2021-05-07 16:35:59
tags:
 - leetcode
 - LFU
categories:
 - leetcode
mathjax:
---

# LFU算法设计

## 机制简介

根据访问频度进行淘汰的缓存淘汰机制，每次淘汰那些使用次数最少的数据。

当遇到多个访问次数相同时，删除最早插入的那个。

<!-- more -->

## 思路分析

根据LFU的算法逻辑，其需要满足以下几个条件：

1. 调用`get(key)`方法时，要返回`key`对应的`val`。
2. 只要用`get`或者`put`方法访问一次某个`key`，该`key`的`freq`就要加一。
3. 如果在容量满了之后继续插入，则需要将`frep`最小的`key`删除，如果最小的`freq`对应着多个`key`，则删除其中最旧的那个。

因为我们需要在`O(1)`的时间复杂度内完成以上操作，所以下面对以上的条件进行具体分析：

1. 使用一个`HashMap`存储`key`到`val`的映射，就可以快速计算`get(key)。`
   `HashMap<Integer, Integer> keyToVal;`
2. 使用一个`HashMap`存储`key`到`freq`的映射，就可以快速操作`key`对应的`freq`。
   `HashMap<Integer, Integer> keyToFreq;`
3. 该部分为LFU算法的核心，分为以下几点:
   1. 需要`freq`到`key`的映射
   2. 将`freq`最小的`key`删除，那么就应该能够快速得到当前所有`key`最小的`freq`是多少。想要时间复杂度`O(1)`的话，肯定不能遍历，需要使用一个遍历`minFreq`来记录当前最小的`freq`。
   3. 可能有多个`key`拥有相同的`freq`，所以`freq`对`key`是一对多的关系，一个`freq`对应着一个`key`的列表
   4. 希望`freq`对应的`key`的列表是存在时序的，便于快速查找并删除最旧的`key`。
   5. 希望能够快速删除`key`列表中的任何一个`key`，因为如果频次为`freq`的某个`key`被访问，那么它的频度就会变为`freq+1`，就应该从`freq`对应的`key`列表中删除，加到`freq+1`对应的`key`列表里。
      `HashMap<Integer, LinkedHashSet<Integer>> freqToKeys;`
      `int minFreq = 0;`

基本数据结构：

```java
import java.util.HashMap;
import java.util.LinkedHashSet;


public class LFUCache {
    //key到val的映射
    HashMap<Integer, Integer> keyToVal;
    //key到freq的映射
    HashMap<Integer, Integer> keyToFreq;
    //freq到key表的映射
    HashMap<Integer, LinkedHashSet<Integer>> freqToKey;
    //记录最小的频次
    int minFreq;
    //记录LFU缓存的最大容量
    int cap;

    public LFUCache(int capacity){
        keyToVal = new HashMap<>();
        keyToFreq = new HashMap<>();
        freqToKey = new HashMap<>();
        cap = capacity;
        minFreq = 0;
    }

    public int get(int key){}

    public void put(int key, int value){}
}

```

## 代码框架

```java
import java.util.HashMap;
import java.util.LinkedHashSet;

public class LFUCache {
    //key到val的映射
    HashMap<Integer, Integer> keyToVal;
    //key到freq的映射
    HashMap<Integer, Integer> keyToFreq;
    //freq到key表的映射
    HashMap<Integer, LinkedHashSet<Integer>> freqToKeys;
    //记录最小的频次
    int minFreq;
    //记录LFU缓存的最大容量
    int cap;

    public LFUCache(int capacity){
        keyToVal = new HashMap<>();
        keyToFreq = new HashMap<>();
        freqToKeys = new HashMap<>();
        cap = capacity;
        minFreq = 0;
    }

    public int get(int key){
        if(!keyToVal.containsKey(key)){
            return -1;
        }
        //访问一次增加key对应的频度
        increaseFreq(key);
        return keyToVal.get(key);
    }

    public void put(int key, int value){
        if(cap <= 0) return;
        //若key已经存在，则修改对应的val即可
        if(keyToVal.containsKey(key)){
            keyToVal.put(key, value);
            //对应的频度加一
            increaseFreq(key);
            return;
        }
        //key不存在，需要插入
        //容量义满的话需要淘汰一个freq最小的key
        if(cap <= keyToVal.size()){
            removeMinFreqKey();
        }
        //插入key和val，对应的freq为一
        //插入KV表
        keyToVal.put(key, value);
        //插入KF表
        keyToFreq.put(key, 1);
        //插入FK表
        freqToKeys.putIfAbsent(1, new LinkedHashSet<>());
        freqToKeys.get(1).add(key);
        //插入新key后最小频度为1
        minFreq = 1;
    }

    private void increaseFreq(int key){
        int freq = keyToFreq.get(key);
        //更新KF表
        keyToFreq.put(key, freq + 1);
        //更新FK表
        //将key从freq对应的列表中删除
        freqToKeys.get(freq).remove(key);
        //将key加入freq+1对应的列表中
        freqToKeys.putIfAbsent(freq + 1, new LinkedHashSet<>());
        freqToKeys.get(freq + 1).add(key);
        //如果freq对应的列表空了，移除这个freq
        if(freqToKeys.get(freq).isEmpty()){
            freqToKeys.remove(freq);
            //如果这个freq恰好是minFreq，更新minFreq
            if(freq == minFreq){
                minFreq++;
            }
        }
    }

    private void removeMinFreqKey(){
        //获取频度最小的列表
        LinkedHashSet<Integer> keyList = freqToKeys.get(minFreq);
        //最先被插入的那个key就是被淘汰的key
        int deletedKey = keyList.iterator().next();
        //更新Fk表
        keyList.remove(deletedKey);
        if(keyList.isEmpty()){
            freqToKeys.remove(minFreq);
            //此处没有必要更新minFreq，因为remove是在put之前的，put会更新minFreq
        }
        //更新KV表
        keyToVal.remove(deletedKey);
        //更新KF表
        keyToFreq.remove(deletedKey);
    }
}
```



