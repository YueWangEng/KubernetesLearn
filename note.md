### note for k8s learn

1. cron, crontab, cronjob
2. Controller: Deployment, ReplicaSet, StatefulSet, DaemonSet, CronJob.
3. 命令行生成yaml file:    
   kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml
4. resource, resource quota.
5. 判定static pod
   1) 查看Pod的名称和命名空间: Static Pod通常运行在kube-system命名空间, 它们的名称通常会有节点名称作为后缀。
   2) 检查Pod的注解， 它的注解中会包含kubernetes.io/config.source: file或kubernetes.io/config.source: http。
   3) Pod的描述信息： kubectl describe pod <pod-name> -n <namespace>，有Controlled By:  Node/kubelet或者Node/controlplane。
   4) kubelet的配置： 通常位于/etc/kubernetes/manifests，如果在这个目录下有Pod的定义文件，那么这个Pod就是一个Static Pod。
      或者运行kubectl get pods <pod-name> -n <namespace> -o yaml命令，kind: Node。
6. static pod的配置文件一般在/etc/kubernetes/manifests    
   但也有特殊情况，这种情况下，要找到文件位置，需要查看config.yaml（该文件在/var/lib/kubelet中，不一定）。
