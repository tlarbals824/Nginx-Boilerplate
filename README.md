# Nginx Boilerplate

## 참고

https://github.com/wmnnd/nginx-certbot

## 1. init-letsencrypt.sh 가져오기


```bash
# nginx 디렉터리
curl -L https://raw.githubusercontent.com/tlarbals824/nginx-boilerplate/main/init-letsencrypt.sh > init-letsencrypt.sh
```

## 2. init-letsencrypt.sh 설정하기

```bash
# init-letsencrypt.sh

domains=(example.org www.example.org) # 도메인 입력
rsa_key_size=4096
data_path="./data/certbot"
email="" # 이메일 입력
```


## 3. nginx 설정 파일 추가 - 인증서 발급 받기 전

* nginx/data/nginx 디렉터리, nginx.conf 파일 생성

```bash
events{
    worker_connections 1024;
}

http{
    server {
        listen 80;
        server_name example.org;
        server_tokens off;

        location /.well-known/acme-challenge/ {
            root /var/www/certbot;
        }

        location / {
            return 301 https://$host$request_uri;
        }
    }
}

```


## 4. 인증서 발급 받기

```bash
docker compose up -d # nginx 실행 잘되는지 확인

sudo chmod +x init-letsencrypt.sh
sudo ./init-letsencrypt.sh
```

## 5. nginx 설정 파일 추가 - 인증서 발급 받은 후

```bash
events{
    worker_connections 1024;
}

http{
    server {
        listen 80;
        server_name example.org;
        server_tokens off;

        location /.well-known/acme-challenge/ {
            root /var/www/certbot;
        }

        location / {
            return 301 https://$host$request_uri;
        }
    }

    server {
        listen 443 ssl;
        server_name example.org;
        server_tokens off;

        ssl_certificate /etc/letsencrypt/live/example.org/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/example.org/privkey.pem;
        include /etc/letsencrypt/options-ssl-nginx.conf;
        ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

        location / {
            proxy_pass  http://example.org;
        }
    }
}
```