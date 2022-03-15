# cvat-configuration

configure [cvat tool](https://github.com/openvinotoolkit/cvat) with auto annotation option in windows 10 and solve problems in configuration phase.

## Prerequistes Installation:

1- install Docker Desktop<br />
2- Install wsl2<br />
3- ubuntu 18.04 from microsoft store<br />
Hint: Check if Wsl default Version is 2 : open power shell (as administrator) and run command: wsl --set-default-version 2<br />

### Docker Configuration with ubuntu:

1- open docker Desktop, then settings <br/>
a- In general check if **Expose daemon on tcp without TLS** is **Enabled**, and **use docker compose V2** is **Disabled** <br/>
b- In Resourses, Enable **Enable Integration with my default WSL distro**, and **ubuntu 18.04** Enabled as addtional distros<br/>
c- Restart Docker Desktop <br/>

### CVAT configuation:

1- open ubuntu.<br/>
2- Run command : git clone https://github.com/opencv/cvat <br/>
3- cd cvat<br/>
4- Run docker command: docker-compose -f docker-compose.yml -f components/serverless/docker-compose.serverless.yml up -d<br/>
5- cd ..
6-install nuclio with cvat supported version 1.5.16: wget https://github.com/nuclio/nuclio/releases/download/1.5.16/nuctl-1.5.16-linux-amd64 <br/>
7- sudo chmod +x nuctl-1.5.16-linux-amd64
8- sudo chmod +x nuctl-1.5.16-linux-amd64
9- here we need to change in function.yaml file:
a- Go to Dir :Cd cvat/serverless/openvino/omz/public/mask_rcnn_inception_resnet_v2_atrous_coco/nuclio
b- press i for inserting.
7- sudo chmod +x nuctl-1.5.16-linux-amd64<br/>
8- sudo chmod +x nuctl-1.5.16-linux-amd64<br/>
9- here we need to change in function.yaml file:<br/>
a- Go to Dir :Cd cvat/serverless/openvino/omz/public/mask_rcnn_inception_resnet_v2_atrous_coco/nuclio.<br/>
b- press i for inserting.<br/>
c- check attribute node that under trigger node and
add new attribite \*Port: **with any number above 1000\***
add new attribite \*Port: **with any number above 1000\*** .<br/>
d- check attributes node under platform node and add new attributte \*network: **cvat_cvat\***
d- press ESC then **:wq**
10- back to cvat dir : cd ../../../../../..
12- open localhost with 8080 port , 8070 and check every thing working.
11- create new nuclio project: nuctl create project cvat
12- deploy function: for(e.g) deploying mask Rcnn:
nuctl deploy --project-name cvat \
 --path serverless/openvino/omz/public/mask_rcnn_inception_resnet_v2_atrous_coco/nuclio \
 --volume `pwd`/serverless/common:/opt/nuclio/common \
 --platform local
13- wait until deploying completed ,then open cvat, creat task and use auto annotation.
e- press ESC then **:wq**
10- back to cvat dir : cd ../../../../../.. <br/>
11- open localhost with 8080 port , 8070 and check every thing working.<br/>
12- create new nuclio project: nuctl create project cvat.<br/>
13- deploy function: for(e.g) deploying mask Rcnn:<br/>
nuctl deploy --project-name cvat \<br/>
--path serverless/openvino/omz/public/mask_rcnn_inception_resnet_v2_atrous_coco/nuclio \<br/>
--volume `pwd`/serverless/common:/opt/nuclio/common \<br/>
--platform local<br/>
13- wait until deploying completed ,then open cvat, creat task and use auto annotation.<br/>
