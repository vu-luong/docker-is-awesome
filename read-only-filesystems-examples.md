### 1. Create and start a container from the WordPress using --read-only:

```
	docker run -d --name wp --read-only wordpress:5.0.0-php7.2-apache
```

### 2. Check if a container is running using `docker inspect` command:

```
	docker inspect --format "{{.State.Running}}" wp
```

### 3. Check where Apache changed the container's filesystem:

```
	docker run -d --name wp_writable wordpress:5.0.0-php7.2-apache
	docker container diff wp_writable
```

### 4. Allow read-only container to write to `/run/apache2/`:

```
	docker run -d --name wp2 --read-only -v /run/apache2/ \
		--tmpfs /tmp wordpress:5.0.0-php7.2-apache
	docker logs wp2
```

### 5. Create and run Mysql container:

```
	docker run -d --name wpdb -e MYSQL_ROOT_PASSWORD=12345678 mysql:5.7
```

### 6. Create another wp container linked to db container:

```
	docker run -d --name wp3 --link wpdb:mysql -p 8000:80 \
		--read-only -v /run/apache2/ --tmpfs /tmp \
		wordpress:5.0.0-php7.2-apache
```

### 7. Bash script:

```
#!/bin/sh

DB_CID=$(docker create -e MYSQL_ROOT_PASSWORD=12345678 mysql:5.7)
docker start $DB_CID

MAILER_CID=$(docker create dockerinaction/ch2mailer)
docker start MAILER_CID

WP_CID=$(docker create --link $DB_CID:mysql -p 8000:80 \
	--read-only -v /run/apache2/ --tmpfs /tmp \
	wordpress:5.0.0-php7.2-apache)
docker start $WP_CID

AGENT_CID=$(docker create --link $WP_CID:insideweb \
	--link $MAILER_CID:insidemailer \
	dockerinaction/ch2_agent)
docker start $AGENT_CID
```
