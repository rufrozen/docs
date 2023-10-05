## bash ENV
```
export http_proxy=http://127.0.0.1:3128
export https_proxy=http://127.0.0.1:3128
export ftp_proxy=http://127.0.0.1:3128
```

## Centos yum

#### Config file:
```
vi /etc/yum.conf
# file content:
proxy=http://127.0.0.1:3128
```

## Ubuntu apt

#### Config files:
```
vi /etc/apt/apt.conf.d/proxy.conf
# file content:
Acquire {
  HTTP::proxy "http://127.0.0.1:3128";
  HTTPS::proxy "http://127.0.0.1:3128";
  HTTPS::Verify-Peer "false";
  HTTPS::Verify-Host "false";
  HTTP::proxy::mirrors.internal.com DIRECT;
}
```

## wget

#### Config files:
```
# files:
vi /etc/wgetrc
vi ~/.wgetrc

# file content:
use_proxy=yes
http_proxy=http://127.0.0.1:3128
https_proxy=http://127.0.0.1:3128
check_certificate=off
```

## curl
#### Config files:
```
vi ~/.curlrc
# file content:
insecure
```

## pip

#### Config files:
```
# files:
vi ~/.pip/pip.conf
vi /etc/pip.conf
C:/users/{user}/pip/pip.ini

# file content for internal mirror:
[global]
trusted-host=mirror.internal.com
index-url=http://mirror.internal.com/pypi/simple/

# file content for proxy:
[global]
proxy = http://127.0.0.1:3128
trusted-host=files.pythonhosted.org
```

## docker

#### allow insecure docker registries
```
vi /etc/docker/daemon.json
# file content:
{
    "insecure-registries" : [
        "https://ssl.example.com",
        "http://example.com"
    ]
}
```

#### proxy for docker registries
```
mkdir /etc/systemd/system/docker.service.d

vi /etc/systemd/system/docker.service.d/http-proxy.conf
# file content:
[Service]
Environment="HTTP_PROXY=http://127.0.0.1:3128"
Environment="HTTPS_PROXY=http://127.0.0.1:3128"
Environment="NO_PROXY=localhost,127.0.0.1"

# restart docker
systemctl daemon-reload
systemctl restart docker
```
