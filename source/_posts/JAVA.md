layout: hashtable
title: Hashtable vs Hashmap vs Hashset in JAVA
date: 2016-09-12 10:16:42
tags: JAVA
---
##Both HashTable and HashMap implements Map interface.

##Hashtable

Hashtable is basically a data structure to retain values of key-value pair.

It does not allow null for both key and value. It will throw NullPointerException.
Hashtable does not maintain insertion order. The order is defined by the Hash function. So only use this if you do not need data in order.
It is synchronized. It is slow. Only one thread can access in one time.
HashTable rea thread safe.
HashTable uses Enumerator to iterate through elements.

##HashMap

Like Hashtable it also accepts key value pair.

It allows null for both key and value.
HashMap does not maintain insertion order. The order is defined by the Hash function.
It is not synchronized. It will have better performance.
HashMap are not thread safe, but you can use Collections.synchronizedMap(new HashMap<K,V>())


##HashSet

HashSet does not allow duplicate values.

It provides add method rather put method.
You also use its contain method to check whether the object is already available in HashSet. HashSet can be used where you want to maintain a unique list.
