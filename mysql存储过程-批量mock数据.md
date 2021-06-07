<pre>

-- 方式1.repeat
-- 定义结束符为$
delimiter $
-- 删除已经存在的存储过程
DROP procedure IF EXISTS `p_batch_insert`$
-- 创建存储过程,定义存储方法
create procedure p_batch_insert(in args int)
begin
 declare i int default 1;
 -- 开启事务(重要!不开的话,100w数据需要论天算)
 start transaction;
 repeat
  -- 创建数据
  insert into xxx(...) value(concat('test',i));
  -- 递增1
  set i = i+ 1;
	until i <= args
 end repeat;
 commit;
end$
-- 还原结束符为;
delimiter ;

-- 方式2.do-while
-- 定义结束符为$
delimiter $
-- 删除已经存在的存储过程
DROP procedure IF EXISTS `p_batch_insert`$
-- 创建存储过程,定义存储方法
create procedure p_batch_insert(in args int)
begin
 declare i int default 1;
 -- 开启事务(重要!不开的话,100w数据需要论天算)
 start transaction;
 while i <= args do
  -- 创建数据
  insert into xxx(...) value(concat('test',i));
  -- 递增1
  set i = i+ 1;
 end while;
 commit;
end$
-- 还原结束符为;
delimiter ;

-- 调用存储过程,生成100W条数据
call p_batch_insert(1000000);

<pre>
