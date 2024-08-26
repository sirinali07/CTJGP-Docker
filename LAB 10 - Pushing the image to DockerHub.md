## Pushing the image to DockerHub
```
docker login
```
```
docker pull ubuntu
```
Make sure to replace `sirinali07` with the name of your DockerHub repo and `version1` with the tag of your choice
`docker tag <image:tag> <dockerhub-account-id>/<imagename:tag>`
```
docker tag ubuntu:version1 sirinali07/ubuntu:version2
```
```
docker image ls
```
```
docker push sirinali07/ubuntu:version2
```
```
docker image ls
```
```
docker image rm sirinali07/ubuntu:version2   #local repo
```
```
docker run -d -p 8080:80 sirinali07/ubuntu:version2
```
```
docker ps -a
```
```
docker container rm < replace container id/name >
```
```
docker image ls
```
Remove multiple containers with a single command
```
docker image rm < replace image id/name >  < replace image id1/name1 >
```
```
docker image ls
```

