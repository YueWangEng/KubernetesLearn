### note for k8s learn

1. cron, crontab, cronjob
2. Controller: Deployment, ReplicaSet, StatefulSet, DaemonSet, CronJob.
3. 命令行生成yaml file:    
   ```
   kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml
   ```
4. resource, resource quota.
5. 判定static pod
   1) 查看Pod的名称和命名空间: Static Pod通常运行在kube-system命名空间, 它们的名称通常会有节点名称作为后缀。
   2) 检查Pod的注解， 它的注解中会包含kubernetes.io/config.source: file或kubernetes.io/config.source: http。
   3) Pod的描述信息： kubectl describe pod <pod-name> -n <namespace>，有Controlled By:  Node/kubelet或者Node/controlplane。
   4) kubelet的配置： 通常位于/etc/kubernetes/manifests，如果在这个目录下有Pod的定义文件，那么这个Pod就是一个Static Pod。
      或者运行kubectl get pods <pod-name> -n <namespace> -o yaml命令，kind: Node。
6. static pod的配置文件一般在/etc/kubernetes/manifests    
   但也有特殊情况，这种情况下，要找到文件位置，需要查看config.yaml（该文件在/var/lib/kubelet中，不一定）。
7. 在 Pod 的 YAML 文件中，可以在 spec 部分中使用 command 字段来定义容器的命令。用于指定容器启动时要执行的命令，通常用于覆盖容器镜像中默认的启动命令。    
   方式一：command 字段应该是一个字符串数组，其中第一个元素是要执行的命令，后续元素是命令的参数。数组可以采用 
   ```
         ['command', 'arg1', ...]
         或者
          -'command'
          - ‘arg1'
          - '...'
   ```
   方式二： 除了在 command 字段中直接指定命令外，您还可以使用 args 字段来为容器传递参数。
8. 直接用语句修改arg (不修改command）
   ```
   kubectl run webapp-green --image=kodekloud/webapp-color -- --color=green
   ```
9. 区分ConfigMaps（一种资源对象，用于存储配置数据）和manifest（清单文件,资源对象的定义文件）
10. Kubernetes 中的 ConfigMap 和 Secret 的格式相似。  
    ConfigMap 用于存储非敏感的配置数据，是明文存储的。Secret 用于存储敏感的信息，是加密存储的。
