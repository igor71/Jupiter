# TensorFlow Object Detection With Ubuntu Docker

## Create docker image with TensorFlow and run object detection example.

We will use Ubuntu image as base, for that we should extend our new image from ubuntu official repository:

https://hub.docker.com/_/ubuntu/

```
As we are going to run object detection example we need to install all dependencies. 
Tensorflow Object Detection API depends on the following libraries:
Protobuf
Pillow
lxml
tfSlim (which is included in the “tensorflow/models/research/” checkout)
Jupyter notebook
Matplotlib
Tensorflow version >= 1.12.0
```

The Tensorflow Object Detection API uses Protobufs to configure model and training parameters. Before the framework can be used, the Protobuf libraries must be compiled. This should be done by running the following command:

`protoc object_detection/protos/*.proto --python_out=.`

When running locally, the /tensorflow/models/research/ and slim directories should be appended to PYTHONPATH. This can be done by running the following command:

`export PYTHONPATH=$PYTHONPATH:`pwd`:`pwd`/slim`

In order to ignore deprecation warnings in Python commnds we are using following option:

`PYTHONWARNINGS=ignore:DEPRECATION pip ...`


### Build Jupiter-Notebook Docker Image Manually
```
git clone --branch=Jupiter-Tensorflow --depth=1 https://github.com/igor71/Jupiter.git

cd Jupiter

docker build -f Dockerfile -t yi/jupiter-tensorflow:latest .`
```

### Running Docker Container
```
docker run -d --name $USER-jupiter-tf -p 8889:8889 yi/jupiter:latest
```

### Access Jupiter Notebook
```
http://server-6:8889

yi-dockeradmin <username>-jupiter-tf
```

### Reference:
https://towardsdatascience.com/tensorflow-object-detection-with-docker-from-scratch-5e015b639b0b
