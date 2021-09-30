```
  1.创建用户test
  useradd test (创建用户test,组test; 也可以 groupadd test & useradd test -g test)
  2.设置用户test密码
  passwd test
  3.查看已经创建的用户test
  cat /etc/passwd | grep test
  4.控制某个文件的访问权限
  setfacl -m u:test:rwx a.txt
  5.获取某个文件的访问权限
  getfacl a.txt
  
  ------------------
  改变目录访问权限
  chmod [ugoa, u文件拥有者g与文件拥有者同一组内的人o其他用户a所有人][+-=, +拥有-取消＝指定] [rwx, r读w写x执行] [-R 包括子目录] [目标]
  改变拥有者
  chown [用户][:组] [目标]
```
