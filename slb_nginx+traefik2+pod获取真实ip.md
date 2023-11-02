```
1.slb开启X-Forwarded-For获取真实ip

2.[nginx]
proxy_set_header X-real-ip $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

3.[traefik2] https://doc.traefik.io/traefik/routing/entrypoints/
entryPoints:
  name:
    address: ":8888" # same as ":8888/tcp"
    proxyProtocol:
      insecure: true
    forwardedHeaders:
      insecure: true
or
--entryPoints.name.address=:8888 # same as :8888/tcp
--entryPoints.name.proxyProtocol.insecure=true
--entryPoints.name.forwardedHeaders.insecure=true

4. request.getHeader("X-Forwarded-For");
```
