# ansible-role-kubernetes-lb

Load balancer for Kubernetes control plane

## Install

```shell
ansible-galaxy role install git+https://github.com/sergelogvinov/ansible-role-kubernetes-lb.git,main
```

## Use

```yaml
haproxy_vhost: |
  frontend contolplane
    bind 127.0.0.1:6443
    use_backend contolplane

  backend contolplane
    mode tcp
    fullconn 512
    balance roundrobin

    option  httpchk GET /readyz HTTP/1.0
    http-check expect status 200

    default-server verify none check-ssl inter 15s fall 2 rise 2 on-marked-down shutdown-sessions resolvers resolvconf init-addr last,none resolve-prefer ipv4
    server api1 172.16.1.11:6443 check
    server api2 172.16.1.12:6443 check
    server api3 172.16.2.11:6443 backup check
    server api4 172.16.2.12:6443 backup check
```
