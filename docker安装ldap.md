docker 安装参考

https://github.com/moowcharnfu/dev-docs/blob/master/docker%E5%AE%89%E8%A3%85.md

docker 安装 ldap

https://www.jakehu.me/2020/ldap-docker/

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
