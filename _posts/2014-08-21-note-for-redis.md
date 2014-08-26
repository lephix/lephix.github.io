---
layout: post
title: Note for Redis
---

Redis is an open source, BSD licensed, advanced key-value cache and store. It is often referred to as a data structure server since keys can contain strings, hashes, lists, sets, sorted sets, bitmaps and hyperloglogs.



Redis official website: [redis.io](http://www.redis.io)

Try redis with your web browser [try.redis.io](http://try.redis.io)

**Redis cannot use these types recursively.**

## Data types

### String

A basic type for redis. 

~~~bash
> set testkey value
OK
> get testkey
"value"
~~~

Bit operation is also supported.

~~~bash
> setbit test 4 1
0
> getbit test 4
1
> bitcount test
1
> setbit test1 3 1
0
> bitcount test1
1
> bitop OR result test test1
1
> getbit result 3
1
> getbit result 4
1
~~~

## Hashes

This type can contain number of fields for a object. 

~~~bash
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
~~~

### Lists

Lists is implemented with a bidirection links. So it is very fast to get a item at the end of both side. 

~~~bash
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
~~~

### Sets

Sets is implemented with a no-value Hashes. No identical items exist in Sets. SDIFF, SINTER, SUNION can be used for set operation.

~~~bash
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
~~~

### Sorted Sets

Sorted Sets is implemented by skip list. The element's score is supportting integer, double and (+inf means + infinity, -inf means - infinity). So the time complexity is O(log(N)).

~~~bash
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
~~~

## Advanced Topics

### Transaction

Use command `MULTI` to start a transaction, and use command `EXEC` to execute all the queued commands in the transaction. If one of command have a runtime error (like mismatching the command with the type of the key), other command will still be executed. There is no Roll-Back feature in Redis.

~~~bash
> set a 1
OK
> set b 2
OK
> multi
OK
> set a 2
QUEUED
> hset b name 1
QUEUED
> exec
1) OK
2) WRONGTYPE Operation against a key holding the wrong kind of value
> get a
"2"
~~~

### Expiration

Use `EXPIRE` to set a expiration for a key, and `TTL` to get the time to live for a key.

~~~bash
> set a 1
OK
> expire a 10
(integer) 1
> ttl a
(integer) 3
> get a
(nil)
~~~

### Sort

Use `SORT` to sort a Lists. This command is the most complex command currently. With parameter `DESC`, `BY` and `GET`, can customize the sort order, reference of sort key and returning data. `STORE` could store the result into another Lists object, instead of output directly. The time complexity is O(n+mlogm), n is the total data going to be sorted, m is the number of returning data.

~~~bash
> lrange tag:ruby:posts 0 -1
1) "6"
2) "8"
3) "10"
4) "2"
5) "1"
> hgetall post:6
1) "time"
2) "6"
> SORT tag:ruby:posts BY post:*->time DESC GET post:*->title GET # STORE sort.result
5
> lrange sort.result 0 -1
1) "10"
2) "8"
3) "6"
4) "2"
5) "1"
~~~

### Blocking operation

`BRPOP` could be blocked until the Lists has a element in it. `BRPOP key 0` could be blocked until there is another client `LPUSH key 1`. 0 means block forever. `BLPOP` is provided also. 

This command can monitor at multiple keys. `BRPOP key1 key2 0`. key1 will be checked before key2.

### Publish / Subscribe

Client could publish message to channel, and subscribe channel for receiving messages. If in the subscribe mode, client could only execute `subscribe, unsubscribe, psubscribe, punsubscribe` commands. p means pattern. Only when no more channel subscribed, client will be out of the subscribe mode.

