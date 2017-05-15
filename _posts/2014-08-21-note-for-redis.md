---
# layout: post
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

### Hashes

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

### Internal storage

Every value in Redis is a redisObject.

~~~C
typedef struct redisObject {
	unsigned type:4;
	unsigned notused:2; /* Not Used */
	unsigned encoding:4;
	unsigned lru:22; /* lru time (relative to server.lruclock) */
	int refcount;
	void *ptr;
} robj;
~~~

type and encoding could be one of following.

~~~C
/* type value */
#define REDIS_STRING 0
#define REDIS_LIST 1
#define REDIS_SET 2
#define REDIS_ZSET 3
#define REDIS_HASH 4

/* encoding value */
#define REDIS_ENCODING_RAW 0
#define REDIS_ENCODING_INT 1
#define REDIS_ENCODING_HT 2
#define REDIS_ENCODING_ZIPMAP 3
#define REDIS_ENCODING_LINKEDLIST 4
#define REDIS_ENCODING_ZIPLIST 5
#define REDIS_ENCODING_INTSET 6
#define REDIS_ENCODING_SKIPLIST 7
~~~

#### String

Redis use sdshdr for storing a String value if the value of the string . `ptr` in redisObject point to the address of it.

~~~C
struct sdshdr {
	int len; /* length of the string */
	int free; /* free space in buff */
	char buff[];
}
~~~

During the startup, Redis will create 10000 redisObjects that represent values from 0 to 9999 as shared redisObjects. This optimization could be used for everywhere that use redisObject.If the value of the string is between 0 - 9999, key will point to the shared redisObjects instead of create a new one.

If `maxmemory` is set in configuration file, no shared redisObjects will be used. Because each value need the LRU information.

#### Hashes

REDIS_ENCODING_HT or REDIS_ENCODING_ZIPLIST will be used for Hashes, it depends on the following configurations. If the amount of fields less than `hash-max-ziplist-entries` and all the fields name length and fields value length are less than `hash-max-ziplist-value`, ZIPLIST will be used, otherwise HT will be used.

~~~properties
hash-max-ziplist-entries 512 
hash-max-ziplist-value 64 /* bytes */
~~~

ZIPLIST use a compact data struct for saving data space extremely. However, it will sacrifice the search speed, rearrange the data when data changes. so those two properties' value must be small.

#### Lists

Redis will use REDIS_ENCODING_INTSET instead of REDIS_ENCODING_HT when all the values in Lists is a Integer and the amount of values is less than `set-max-intset-entries` in configuration file. If the condition is not fulfilled, Redis will change to use REDIS_ENCODIING_HT, and will not change back cause the high performance of monitoring. `intset` stores data in numeric order, so it has high performance in searching, but low performance in manipulating. Following is the `intset` definition.

~~~C
typedef struct intset {
	uint32_t encoding;
	uint32_t length;
	int8_t contents[];
} intset;
~~~

#### Sorted Sets

Sorted Sets use REDIS_ENCODING_ZIPLIST or REDIS_ENCODING_SKIPLIST as internal storage. The condition for the transition is as the same as Hashes.

If Redis using REDIS_ENCODING_SKIPLIST, Hashes will be used for storing the mapping relationship between elements value and elements score, So `ZSCORE` could be applied within O(1). And use SkipList for storing element score and the mapping to element value. Redis changed some default SkipList behavior, allow same score element, adding a pointer to point previous element. Element value is stored as a RedisObject, so shared RedisObject could be helpful. Element score is stored as double.


## Script

### Description

Redis provide support for Lua script after 2.6. There are some advantages for script.

1. Reduce the network overhead. Only one command will be send to Redis.
2. Atomic operation. Redis will take the script as a atomic operation, no need to take care of the concurrency.
3. Reused for all the client. Because script is stored in Redis server, so all the client could leverage this script.

### Practice

Following is a script named "ratelimiting.lua".

~~~Lua
local time=redis.call('incr', KEYS[1])
if times==1 then  -- kyes[1] just created, so set the expiration for it.
redis.call('expire', KEYS[1], ARGV[1])
end

if times > tonumber(ARGV[2]) then
return 0
end

return 1
~~~

We could use this script with following command.

~~~bash
$redis-cli --eval /path/to/ratelimiting.lua rate.limiting:127.0.0.1 , 10 3
~~~

## Management

### Backup

#### RDB

RDB is the default way to persist the data in memory. RDB will be trigger by scheduler or manually. `SAVE` and `BGSAVE` are the commands for persisting memory. `SAVE` will use main process to persist data, `BGSAVE` will fork a new process to persist data. 

Following is the configuration for auto persisting data.

~~~
save 900 1 # means if more than 1 key is changed in 900 seconds (15 minutes), will trigger the persist operation.
save 300 10
~~~

1. RDB will use `fork()` to create new process for persisting data in memory. 
2. Parent process will continue to serve clients. Child process will write memory data into a temporary RDB file. (When doing the `fork` in Unix or Linux System, **copy-on-write** strategy will be used. **copy-on-write** means parent and child process share memory, but when parent child do a write operation, system will copy this piece of memory instead of change it directly in the share memory. So this strategy could protect child process from suffering memory inconsistent situation.)
3. After child process finishing the writing operation, new RDB file will replace the old one.

If Redis quit accidentally, data changes after the last backup will get lost.

#### AOF

AOF (append only file) is another way to persist memory to disk. It is disabled by default. Change to `appendonly yes` to enable it. If AOF is enabled, every command sent to Redis for changing data will appended to AOF file. The command will be written to the system cache instead of the disk.

AOF file will contain lots of redundant command. So rewrite AOF file is needed. Following is the configuration for rewriting. `BGREWRITEAOF` could run rewrite manually.

~~~properties
auto-aof-rewrite-percentage 100 # The size percentage of the last aof file, if exceed this percentage, rewrite will be triggered.
auto-aof-rewrite-min-size 64mb # The minimize size of the aof file in current status, if exceed this size, rewrite will be triggered.
~~~

There is another configuration for the synchronization between system cache and disk.

~~~properties
#appendfsync always # means sync every time.
appendfsync everysec # means sync every second.
#appendfsync no # means sync depend on system, default is 30 seconds.
~~~

Redis support enabling RDB and AOF, and will use AOF for restoring data because less data could be lost.

### Cluster

#### Master and slaves

It's very easy to set slaves for a Redis server. Just set `slaveof MASTER-IP MASTER-PORT` in slaves configuration and run it. Slaves node is read-only by default. Set `slave-read-only no` in configuration file will enable client to update data in this specific slave Redis server (won't sync to other servers), but the data will be overwritten by master Redis server during synchronization. 

~~~
> SLAVEOF 127.0.0.1 6379 # could make this redis server as a slave server. If is was a slave for another master, will discard dataset.
> SLAVEOF NO ONE # make this redis server as a master server. This won't discard dataset.
~~~

#### Principle

When a slave server started, will send `SYNC` command to master server. Master server will take a snapshot of current dataset (The same as taking a RDB save), new data changes will be cached. Master will send the snapshot and cache to slave server.

