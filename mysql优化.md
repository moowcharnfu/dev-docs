```
# innodb_buffer_pool_size
show variables like '%max_connection%';

show global status like 'Thread%';

show variables like 'innodb_buffer_pool%';

show status like 'innodb_buffer_pool_read%';

-- 3g 134217728

-- 修改配置文件mysql.conf

-- SET GLOBAL innodb_buffer_pool_size = 3221225472;

-- SHOW STATUS WHERE Variable_name='InnoDB_buffer_pool_resize_status';

-- Performance = innodb_buffer_pool_reads / innodb_buffer_pool_read_requests * 100

SELECT @@innodb_buffer_pool_size/1024/1024/1024; 

select 9376/34339737*100 as '性能';

select 24251/7914185*100 as '性能';

-- InnoDB buffer pool 命中率 = innodb_buffer_pool_read_requests / (innodb_buffer_pool_read_requests + innodb_buffer_pool_reads ) * 100 此值低于99%，则可以考虑增加innodb_buffer_pool_size

select 34339737/(34339737+9376)*100 as '命中率';

select 7914185/(7914185+24251)*100 as '命中率';
```
