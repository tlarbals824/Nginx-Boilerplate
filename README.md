# Nginx Boilerplate


## init-letsencrypt.sh 가져오기


```bash
curl -L https://raw.githubusercontent.com/tlarbals824/nginx-boilerplate/master/init-letsencrypt.sh > init-letsencrypt.sh
```

## init-letsencrypt.sh 설정하기

```bash
# init-letsencrypt.sh

domains=(example.org www.example.org) # 도메인 입력
rsa_key_size=4096
data_path="./data/certbot"
email="" # 이메일 입력
```