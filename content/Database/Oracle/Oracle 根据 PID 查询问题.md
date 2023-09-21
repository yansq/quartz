# Oracle 根据 PID 查询问题

服务器上排查到某个 oracleorcl 进程占用了较多资源；

使用以下 sql 获得 session 相关信息：
```sql
select b.spid,a.sid, a.serial#,a.username, a.osuser, a.sql_id
from v$session a, v$process b
where a.paddr= b.addr
and b.spid='&spid'
order by b.spid;
```

通过上一步查出的 `sql_id` 获取具体的 sql 信息：
```sql
select *
from v$sql
where sql_id = '&sql_id'
```