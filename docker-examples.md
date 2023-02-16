### 1. Download, install and start a container running NGINX

```
	docker run --detach --name web nginx:latest
```

### 2. Run an interactive container:

```
	doker run --interactive --tty --link web:web \
	--name web_test busybox:1.29 /bin/sh
```

### 3. List containers:

```
	docker ps
```

### 4. Restart container by name

```
	docker restart web
```

### 5. Examine the logs for a container:

```
	docker logs web -f
```

### 6. Run additional processes in a running container:

```
	docker run -d --name namespaceA busybox:1.29 /bin/sh -c "sleep 30000"
	docker exec namespaceA ps
```

### 7. Rename container

```
	docker rename old-name new-name
```

### 8. Assign container ID to a shell variable:

```
	CID=$(docker create nginx:latest)
	echo $CID
```

### 9. Write the container ID to a known file:

```
	docker create --cidfile /tmp/web.cid nginx
	cat /tmp/web.cid
```
