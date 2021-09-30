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
```
