# 此工具帮助你拉取墙外的镜像 
## 用法
 `./puller <imagename:tag>` 
 `./puller gcr.io/google_containers/pause-amd64:3.0` 
## 依赖
* Docker
* Git
* Sha1sum 

## 已知问题
* 镜像第一次拉取速度比较慢，建议配置dockerhub 加速
* 在docker ce 17.XX 上不可用