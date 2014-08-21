---
layout: post
title: Note for Redis
---

Redis is an open source, BSD licensed, advanced key-value cache and store. It is often referred to as a data structure server since keys can contain strings, hashes, lists, sets, sorted sets, bitmaps and hyperloglogs.



Redis official website: [redis.io](http://www.redis.io)

Try redis with your web browser [try.redis.io](http://try.redis.io)

**Redis cannot use these types recursively.**

## String

A basic type for redis. 

```bash
> set testkey value
OK
> get testkey
"value"
```

## Hashes

This type can contain number of fields for a object. 

```bash
> hmset testobj name lephix age 30
OK
> hkeys testobj
1) "name"
2) "age"
> hvals testobj
1) "lephix"
2) "30"
> hgetall testobj
1) "name"
2) "lephix"
3) "age"
4) "30"
```

## Lists

Lists is implemented with a bidirection links. So it is very fast to get a item at the end of both side. 

```bash
> lpush testlist first
(integer) 1
> lpush testlist second
(integer) 2
> rpush testlist third
(integer) 3
> lget testlist
> lrange testlist 0 -1
1) "second"
2) "first"
3) "third"
```

## Sets

Sets is implemented with a no-value Hashes. No identical items exist in Sets. SDIFF, SINTER, SUNION can be used for set operation.

```bash
> sadd testsets a b c
(integer) 3
> smembers testsets
1) "b"
2) "c"
3) "a"
> srem testsets c
1
> smembers testsets
1) "b"
2) "a"
> scard testsets
2
```

## Sorted Sets

Sorted Sets is implemented by skip list. The element's score is supportting integer, double and (+inf means + infinity, -inf means - infinity). So the time complexity is O(log(N)).

```bash
> zadd testsortset 100 a 60 b 70 c
(integer) 3
> zrange testsortset 0 -1 withscores
1) "b"
2) 60.0
3) "c"
4) 70.0
5) "a"
6) 100.0
> zadd testsortset 5 a
(integer) 0
> zrange testsortset 0 -1 withscores
1) "a"
2) 5.0
3) "b"
4) 60.0
5) "c"
6) 70.0
> zcard testsortset
3
> zrank testsortset b
1
```

