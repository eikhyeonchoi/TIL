# Amazon linux2 기준 nginx 설치
```
$ sudo amazon-linux-extras install nginx1
```

# CentOS7 기준 nginx 설치
```
# yum에는 외부 저장소가 없기 때문에 외부 저장소 추가
$ cd etc/yum.repos.d/
$ vi nginx.repo

# 아래내용으로 수정
[nginx] 
name=nginx repo 
baseurl=http://nginx.org/packages/centos/7/$basearch/ 
gpgcheck=0 
enabled=1

$ yum install -y nginx
```

# 공통

```
# 443, 80 포트 확인
$ netstat -tnlp 

# 설치 확인용 버전확인
$ nginx -v
```

```
# 시작(2개 중 아무거나 실행)
$ sudo service nginx start
$ sudo systemctl start nginx
```

```
# 확인
$ systemctl status nginx
```

```
# 재부팅시 자동 재시작
$ systemctl enable nginx.service

...
# ec2 80, 443 inbound 넣어주기
```

```
# nginx 설정파일 찾기
$ sudo find / -name nginx.conf
```

```
# proxy
$ sudo find / -name nginx.conf

# 찾고 난 후 내용확인해서 include 확인
http {
    include /etc/nginx/conf.d/*.conf;
}
```

```
$ cd /etc/nginx/conf.d
$ vi 원하는이름.conf

ex) 프록시설정 80 to 8080(spring ...);
server {
        listen 80;
        location / {
                proxy_pass http://localhost:8080;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header Host $http_host;
        }
}

ex) 프록시 설정이 필요 없는경우(react, vue ...)
server {
        listen 80;
        server_name example.com;
        #HTTP 로 접근하면 HTTPS 로 redirect
        return 301 https:#$host$request_uri;
}

#SSL(HTTPS 443 포트) 설정
server {
         listen 443 ssl http2;
        listen [::]:443 ssl http2;

        server_name example.com;
        #인증서 경로
        ssl_certificate /etc/letsencrypt/live/hk/cert.pem;

        #인증키 경로
        ssl_certificate_key /etc/letsencrypt/live/hk/privkey.pem;
        location / {
        root /var/www;
        index index.html;
        try_files $uri /index.html;
}
```