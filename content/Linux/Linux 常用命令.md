# Linux 常用命令

Run history number(e.g. 53)
```bash
!53
```

Run past command that began with (e.g. cat filename)
```shell
!cat
# or
!c
# run cat filename again
```

Bash globbing
```shell
# '*' serves as a "wild card" for filename expansion.
/etc/pa*wd    #/etc/passwd

# '?' serves as a single-character "wild card" for filename expansion.
/b?n/?at      #/bin/cat

# '[]' serves to match the character from a range.
ls -l [a-z]*   #list all files with alphabet in its filename.

# '{}' can be used to match filenames with more than one patterns
ls *.{sh,py}   #list all .sh and .py files
```

Copy and overwrite the target file 
```shell
/bin/cp -rf xxxx
```

Get local ip
```shell
ifconfig -a | grep inet | grep -v 127.0.0.1 | grep -v inet6 | awk '{print $2}' | tr -d "addr:"
```

Find the process id by name
```shell
ps -ef | grep elasticsearch | grep -v "grep" | awk '{print $2}' | head -n 1
```

