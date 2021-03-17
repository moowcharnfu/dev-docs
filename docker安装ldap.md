docker 安装参考

https://github.com/moowcharnfu/dev-docs/blob/master/docker%E5%AE%89%E8%A3%85.md

docker 安装 ldap

https://www.jakehu.me/2020/ldap-docker/

https://www.cnblogs.com/kevingrace/p/5773974.html

-- server

docker pull osixia/openldap

docker run -p 389:389 --name ldap --restart=always \
--env LDAP_ORGANISATION="lam" \
--env LDAP_DOMAIN="lam.com" \
--env LDAP_ADMIN_PASSWORD="lam" \
--env LDAP_CONFIG_PASSWORD="lam" \
--volume /data/database:/var/lib/ldap \
--volume /data/config:/etc/ldap/slapd.d \
--detach osixia/openldap

-- management

docker pull ldapaccountmanager/lam

docker run -d --restart=always --name lam -p 3890:80 \
--link ldap:ldap \
--env LDAP_DOMAIN=lam.com \
--env LAM_LANG=zh_CN \
--env LDAP_SERVER=ldap://ldap:389 \
--env LAM_PASSWORD=lam \
--detach ldapaccountmanager/lam


-- 使用总结

1.创建用户

一）先创建组，设定组名

二）创建用户，选择分组，设置密码

2.密码加密算法

OpenLDAP 支持 CRYPT, MD5, SSHA 和 SHA 四种加密算法保存密码，默认使用 SSHA

由于org.ldaptive.auth.AbstractCompareAuthenticationHandler默认使用SHA加密算法, 故在ldap界面创建密码优先使用SHA避免验证失败


3.ldif文件导入

[ldap导入.ldif]

#创建部门

dn: ou=测试使用中心1,dc=lam,dc=domain

objectclass: organizationalUnit

objectclass: top


dn: ou=产品,ou=测试使用中心1,dc=lam,dc=domain

objectclass: organizationalUnit

objectclass: top


dn: ou=测试,ou=测试使用中心1,dc=lam,dc=domain

objectclass: organizationalUnit

objectclass: top


dn: ou=研发,ou=测试使用中心1,dc=lam,dc=domain

objectclass: organizationalUnit

objectclass: top


#创建用户, 用户名最少两位

dn: uid=李四,ou=产品,ou=测试使用中心,dc=lam,dc=domain

objectclass: inetOrgPerson

objectclass: organizationalPerson

objectclass: person

cn: 李四

sn: 李四

uid: 李四

userpassword: {SHA}fEqNCco3Yq9h5ZUglD3CZJT4lBs=

#manager管理哪些目录,这是用来管理谁

manager: uid=李四,ou=研发,ou=测试使用中心1,dc=lam,dc=domain

#o属于哪个组织, 这里用来做被谁管理

o:admin



dn: uid=李四,ou=测试,ou=测试使用中心,dc=lam,dc=domain

objectclass: inetOrgPerson

objectclass: organizationalPerson

objectclass: person

sn: 李四

uid: 李四

cn: 李四

userpassword: {SHA}fEqNCco3Yq9h5ZUglD3CZJT4lBs=

#manager管理哪些目录,这是用来管理谁

manager: uid=李四,ou=产品,ou=测试使用中心1,dc=lam,dc=domain

#o属于哪个组织, 这里用来做被谁管理

o:admin





######### gitlab支持ldap登陆
vim /home/gitlab/config/gitlab.rb

gitlab_rails['ldap_enabled'] = true

gitlab_rails['ldap_servers'] = YAML.load <<-'EOS'

  main: # 'main' is the GitLab 'provider ID' of this LDAP server
  
    label: 'LDAP'
    
    host: '192.168.4.100'
    
    port: 389
    
    uid: 'uid'
    
    method: 'plain' # "tls" or "ssl" or "plain"
    
    bind_dn: 'cn=admin,dc=lam,dc=domain'
    
    password: 'lam'
    
    allow_username_or_email_login: false
    
    base: 'dc=tcsl,dc=domain'
    
    attributes:
    
      username: ['uid']
      
      email:    ['mail']
      
      first_name: 'sn'
      
 
EOS
