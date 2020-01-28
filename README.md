# Laravel Docker



#### 디렉토리 구조
```dir
project_root
├── docker/
│   ├── mysql/
│   ├── nginx/
│   │   ├── ca/
│   │   ├── conf.d/
│   │   │   └── default.conf
│   │   └── nginx.conf
│   └── php-fpm/
│       ├── Dockerfile 
│       ├── php.ini 
│       └── php.ini-production 
├── app/
│    └── file2/
├── public/
├── dir1/
├── dir2/
├── composer.json
└── docker-compose.yml


    

```

#### 초기 설정

- 각 OS별로 docker 를 설치
- 터미널에서 다음을 실행하여 php-fpm 소스를 재 컴파일
```bash
shell> docker-compose build --force-rm --no-cache 
```
- docker 실행
```bash
shell> docker-compose up -d
```
- php composer install
```bash
shell> docker exec {image_name} sh -c "composer install --prefer-dist"
```

#### nginx 서버 호스트 설정 
```
server {
    listen  80;
    server_name  localhost;
    root /var/www/html/public;

    index index.html index.htm index.php;

    charset utf-8;

    location / {
         try_files $uri $uri/ /index.php?$query_string;
    }

    error_page 404 /index.php;
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }


    location ~ \.php$ {
        root           /var/www/html/public;
        fastcgi_pass   __DOCKER_PHP_FPM__:9000;
       
        try_files $uri =404;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        fastcgi_param SCRIPT_NAME $fastcgi_script_name;
        fastcgi_index index.php;
        include fastcgi_params;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```


#### larabel artisan 명령어
```
shell> docker exec {image_name} sh -c "php artisan route:list"  

shell> docker exec {image_name} sh -c "php artisan route:clear"  

shell> docker exec {image_name} sh -c "php artisan cache:clear"  

shell> docker exec {image_name} sh -c "php artisan optimize"

```

#### larabel routing cache 재생성
```
shell> docker exec {image_name} sh -c "composer dump-autoload"  
```

#### Xdebug 설정 및 사용


----

#### docker 명령어 참조 


```
모든 도커 프로세스 종료
docker stop $(docker ps -a -q)

모든 도커 이미지 삭제
docker rm $(docker ps -a -q)

```
