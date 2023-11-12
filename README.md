## tkim

- 2023/11/12 tkim
- 이 프로젝트는 laravel 10 프로젝트를 aws lambda로 실행해 보기 위한 poc이다.
- 수순은 https://ma-vericks.com/blog/serverless-laravel-app/ 을 참고하였다.
- 환경은 Windows VDI, 

### Project 만들기
```cmd
cd /temp
gh repo create --public laravel-20231112-poc-bref
gh auth refresh -h github.com -s delete_repo
gh repo clone laravel-20231112-poc-bref
gh repo delete laravel-20231112-poc-bref --yes

composer create-project laravel/laravel laravel-20231112-poc-bref
cd laravel-20231112-poc-bref
git init
gh repo create --public laravel-20231112-poc-bref --source=.
```

- tip  
  https://laravel.com/docs/10.x/installation 을 참고하여,
  이후에는 laravel new 명령을 사용하기 위해 `composer global require laravel/installer`를 실행했다.

### Serverless Framework 설치
```cmd
C:\temp\laravel-20231112-poc-bref>npm install -g serverless
C:\temp\laravel-20231112-poc-bref>serverless -v
Framework Core: 3.36.0
Plugin: 7.1.0
SDK: 4.4.0
# from env
C:\temp\laravel-20231112-poc-bref>serverless config credentials --provider aws --key %AWS_ACCESS_KEY_ID% --secret %AWS_SECRET_ACCESS_KEY%
✔ Profile "default" has been configured
C:\temp\laravel-20231112-poc-bref>composer require bref/bref bref/laravel-bridge --update-with-dependencies
C:\temp\laravel-20231112-poc-bref>php artisan vendor:publish --tag=serverless-config
C:\temp\laravel-20231112-poc-bref>cat serverless.yml
# change ap-east-1 to ap-northeast-1
C:\temp\laravel-20231112-poc-bref>php artisan config:clear
C:\temp\laravel-20231112-poc-bref>serverless deploy

Deploying laravel to stage dev (ap-northeast-1)

✔ Service deployed to stack laravel-dev (104s)

endpoint: ANY - https://6lj6wnmcja.execute-api.ap-northeast-1.amazonaws.com
functions:
  web: laravel-dev-web (33 MB)
  artisan: laravel-dev-artisan (33 MB)

Want a better experience than the AWS console? Try out https://dashboard.bref.sh

C:\temp\laravel-20231112-poc-bref>serverless bref:cli --args="--version"
{
    "exitCode": 0,
    "output": "Laravel Framework 10.31.0\n"
}
--------------------------------------------------------------------
Creating storage directories: /tmp/storage/bootstrap/cache, /tmp/storage/framework/cache, /tmp/storage/framework/views, /tmp/storage/psysh
Running 'php artisan config:cache' to cache the Laravel configuration
INFO  Configuration cached successfully.
START
Laravel Framework 10.31.0
END Duration: 102.61 ms (init: 889.84 ms) Memory Used: 107 MB
C:\temp\laravel-20231112-poc-bref>serverless remove
Removing laravel from stage dev (ap-northeast-1)

✔ Service laravel has been successfully removed (31s)

C:\temp\laravel-20231112-poc-bref>
```
