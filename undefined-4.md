# 선착순 이벤트 트래픽 대처



```markdown
brew install docker
brew link docker

docker version
```

docker mysql 실행 명령어

```markdown
docker pull mysql
docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=1234 --name mysql mysql
docker ps
docker exec -it mysql bash
```



docker: no matching manifest for linux/arm64/v8 in the manifest list entries.

오류가 발생하게 되면&#x20;

docker pull -- platform linux/x86\_64 mysql



```markdown
mysql -u root -p
create database coupon_example;
use coupon_example;
```

