# Jupiter

### Build Jupiter-Notebook Docker Image
```
git clone --branch=master --depth=1 https://github.com/igor71/Jupiter.git

cd Jupiter

docker build -f Dockerfile-Jupiter -t yi/jupiter:latest .`
```

### Running Docker Container
```
docker run -d --name $USER-jupiter -p 8889:8889 --env="DISPLAY" -v "/media/DATA_HARD/notebook:/home/ubuntu/notebooks" yi/jupiter:latest
```

### Access Jupiter Notebook
```
http://server-6:8889

http://server-6:8889/lab

yi-dockeradmin <username>-jupiter
```

