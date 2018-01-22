# Docker_images

Backup/Save all Docker Images to a compressed file

docker images | tail -n +2 | grep -v "none" | awk '{printf("%s:%s\n", $1, $2)}' | while read IMAGE; do
	echo $IMAGE
	filename="${IMAGE//\//-}"
	filename="${filename//:/-}.docker-image.gz"
	docker save ${IMAGE} | pigz --stdout --best > $filename
done

-restore
You will later be able to restore it with the docker load command :
gunzip -c jasmin-latest01.docker-image.gz | docker load

-Docker Remove images

Docker RM 
Docker rmi {Images ID}
------------------------------------------------------------------------------------------

docker inspect -f '{{ .Mounts }}' f4c933ef28d1
docker inspect
docker ps
docker inspect f4c933ef28d1
------------------------------------------------------------------------------------------  
docker exec -it jasmin_01 /bin/bash
------------------------------------------------------------------------------------------
docker volume create jasmin-store
docker volume create jasmin-log
docker volume create jasmin-root

docker run -d -p 1401:1401 -p 2775:2775 -p 8990:8990 \
  --name jasmin_10 \
  --mount source=jasmin-store,target=/etc/jasmin/store \
  --mount source=jasmin-log,target=/var/log/jasmin \
  --mount source=jasmin-root,target=/etc/jasmin \
  versionfinal:latest
------------------------------------------------------------------------------------------
--- Bind to Local disk 
 
 docker run -d -p 1401:1401 -p 2775:2775 -p 8990:8990 \
  --name jasmin_13 \
  --mount type=bind,source="$(pwd)"/opt/jasmin/jasmin-store/,target=/etc/jasmin/store \
  --mount type=bind,source="$(pwd)"/opt/jasmin/jasmin-log/,target=/var/log/jasmin \
  --mount type=bind,source="$(pwd)"/opt/jasmin/jasmin-root/,target=/etc/jasmin \
  versionfinal:latest 
 
 ------------------------------------------------------------------------------------------
 
