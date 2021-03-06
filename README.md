# cvat-configuration

configure [cvat tool](https://github.com/openvinotoolkit/cvat) with auto annotation option in windows 10 and solve problems in configuration phase.

## Prerequistes Installation:

1- install Docker Desktop<br />
2- Install wsl2<br />
3- ubuntu 18.04 from microsoft store<br />
Hint: Check if Wsl default Version is 2 : open power shell (as administrator) and run command: wsl --set-default-version 2<br />

### Docker Configuration with ubuntu:

1- open docker Desktop, then settings <br/>

- In general check if **Expose daemon on tcp without TLS** is **Enabled**, and **use docker compose V2** is **Disabled** <br/>
- In Resourses, Enable **Enable Integration with my default WSL distro**, and **ubuntu 18.04** Enabled as addtional distros<br/>
- Restart Docker Desktop <br/>

### CVAT configuation:

1- open ubuntu.<br/>
2- Run command :

```
git clone https://github.com/opencv/cvat
```

3- go to cvat dir

```
cd cvat
```

4- Run docker command:

```
docker-compose -f docker-compose.yml -f components/serverless/docker-compose.serverless.yml up -d
```

5- back to user dir

```
cd ..
```

6-install nuclio with cvat supported version 1.5.16:

```
wget https://github.com/nuclio/nuclio/releases/download/1.5.16/nuctl-1.5.16-linux-amd64
```

7- update permission

```
sudo chmod +x nuctl-1.5.16-linux-amd64
```

8-

```
sudo ln -sf $(pwd)/nuctl-<version>-linux-amd64 /usr/local/bin/nuctl
```

9- here we need to change in function.yaml file:<br/>

- run following command and file will opened using vs-code :

```
code cvat/serverless/openvino/omz/public/mask_rcnn_inception_resnet_v2_atrous_coco/nuclio
```

- add port attributes

```
triggers:
    myHttpTrigger:
      maxWorkers: 2
      kind: 'http'
      workerAvailabilityTimeoutMilliseconds: 10000
      attributes:
        port: 5600 # with any value above 1000
        maxRequestBodySize: 33554432 # 32MB
```

- check attributes node under platform node and add new attributte network: **cvat_cvat**

```
platform:
    attributes:
      restartPolicy:
        name: always
        maximumRetryCount: 3
      mountMode: volume
      network: cvat_cvat
```

- save changes

###

<br/>
10- open localhost with 8080 port , 8070 and check every thing working.<br/>
<br/>
11- create new nuclio project:
``` 
nuctl create project cvat
```
<br/>
12- deploy function: for(e.g) deploying mask Rcnn:<br/>

```
nuctl deploy --project-name cvat \
--path serverless/openvino/omz/public/mask_rcnn_inception_resnet_v2_atrous_coco/nuclio \
--volume `pwd`/serverless/common:/opt/nuclio/common \
--platform local
```

13- wait until deploying completed ,then open cvat, creat task and use auto annotation.<br/>

13r- deploy function: for(e.g) deploying mask Rcnn:

```
nuctl deploy --project-name cvat \
--path serverless/openvino/omz/public/mask_rcnn_inception_resnet_v2_atrous_coco/nuclio \
--volume `pwd`/serverless/common:/opt/nuclio/common \
--platform local
```

15- wait until deploying completed ,then open cvat, creat task and use auto annotation. <br/>

**OR**

1- for CPU deployment:

```
serverless/deploy_gpu.sh serverless/tensorflow/matterport/mask_rcnn <path of function>
```

2- for GPU deployment:

```
serverless/deploy_gpu.sh serverless/tensorflow/matterport/mask_rcnn <path of function>
```
