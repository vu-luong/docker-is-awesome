### 1. See environment variable injection in action:

```
	docker run --env MY_ENVIRONMENT_VAR="this is a test" busybox:1.29 env
```

### 2. Start db and mailer containers shared by different wp clients:

```
	export DB_CID=$(docker run -d -e MYSQL_ROOT_PASSWORD=ch2demo mysql:5.7)
	export MAILER_CID=$(docker run -d dockerinaction/ch2_mailer)
```
