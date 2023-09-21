```shell
PORT=6379
IP=127.0。0.1
PASS=password

for key in $(./redis-cli -p $PORT -h $IP -a $PASS keys \*);
  do
    type=$(./redis-cli -p $PORT -h $IP -a $PASS type $key)
    if [ $type = "hash" ]
    then
      ./redis-cli -p $PORT -h $IP -a $PASS hgetall $key | xargs -d "\n" -I {} echo "\\\"{}\\\"" | xargs -L 2 echo "hset ${key}";
    else
      echo -n "set ${key} "
      ./redis-cli -p $PORT -h $IP -a $PASS get $key;
    fi
    sleep 0.01
  done
```

`sleep 0.01`：防止速度过快导致 Redis 线程阻塞