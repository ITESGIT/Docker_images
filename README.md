# Docker_images

Backup/Save all Docker Images to a compressed file

docker images | tail -n +2 | grep -v "none" | awk '{printf("%s:%s\n", $1, $2)}' | while read IMAGE; do
	echo $IMAGE
	filename="${IMAGE//\//-}"
	filename="${filename//:/-}.docker-image.gz"
	docker save ${IMAGE} | pigz --stdout --best > $filename
done
-
You will later be able to restore it with the docker load command :
gunzip -c jasmin-latest01.docker-image.gz | docker load
