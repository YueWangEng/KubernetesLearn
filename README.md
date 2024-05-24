# KubernetesLearn

1. 生成创建元素的yaml，采用 --dry-run=client -o yaml > pod-redis.yaml    
例如：  
a) 生成元素，run/create  
kubectl run redis --image redis123 --dry-run=client -o yaml   
写入具体文件中   
kubectl run redis --image redis123 --dry-run=client -o yaml > pod-redis.yaml   
b) 修改元素，edit  
有时候，即使无法修改，也没问题，系统会自动生成文件  

2. 修改参数（pod, replicaset等）   
1）修改生成的yaml文件    
2）修改命令：kubectl edit pod/replicaset redis  

3. 生成deployment   
1） 使用yaml   
2）使用kubectl create deployment —help 命令：kubectl create deployment my-dep --image=nginx --replicas=3   

4. ns
   1) kubectl get ns
   2) kubectl get pods —all-namespaces   

5. Create a service redis-service to expose the redis application within the cluster on port 6379.   
kubectl expose pod redis --port=6379 --name=redis-service   

6. 创建对象   
   1）不使用yaml file创建object的时候，只有pod才能用run(且无需只能关键字pod)，别的例如replicates, deployment，需使用create(且需指明关键字)   
   kubectl create deployment webapp --image=kodekloud/webapp-color --replicas=3   
   2）但使用yaml file的时候，都用create，且无需只能关键字pod
   kubectl create –f pod-definition.yml）  

7. kubectl create deployment redis-deploy --namespace=dev-ns  --image=redis --replicas=2   
按照yaml中的顺序，image放在namespace之后   

8. Create a pod called httpd using the image httpd:alpine in the default namespace. Next, create a service of type ClusterIP by the same name (httpd). The target port for the service should be 80.   
kubectl run httpd --image=httpd:alpine --port=80 --expose=true   

9. 检查有没有scheduler   
kubectl get pod -n kube system   

10. 更新yaml以后，重新生成pod，或者将pod从一个node转移到别的node。
    1) 可先删除，再create
    2) 使用 kubectl replace —force -f <yaml>   

11. 根据selector筛选pod，可用   
kubectl get pods --selector (items1),(items2),… (逗号之间不可有空格）   

12. 显示所有内容   
kubectl get all   

13. 小tip   
显示内容时，不限表头，可用例如，kubectl get pods --selector <items> —no headers   
只显示行数，kubectl get pods --selector <items> —no headers | wc -l   

14. taint 是针对node，可用kubectl命令配置   
    toleration是针对pod，只能用yaml配置   

16. 在指定的node部署pod   
    1）yaml中使用nodeName指定
       如nodeName: node01   
    2）给node增加label, pod根据label指定nodeSelector或者affinity:
    ```  
       2.1 对于node，
          可以先用kubectl get node --show-labels查看  
          kubectl label nodes <node-name> <label-key>=<label-value> <label-key>=<label-value> ... (可添加多个标签）
       2.2 对于pod
          2.2.1 yaml使用nodeSelector
          2.2.2 yaml使用Node Affinity,注意key, operator, values之间的配合，例如：operator为in时，values值不可为空。   
    ```
18. 如果要在集群外部访问，可以通过端口转发实现（只适合临时测试用）：   
   kubectl port-forward service/test-k8s 8888:8080

19. 重启pod  
    方法之一： Kubectl replace --force -f (yaml)，不需要delete再create

20. k8s有各种资源对象，它们是Kubernetes API中定义的各种实体，用于表示集群的状态、配置、规范等通常。  
>常见的有：  
>Pods, Services, Deployments, ReplicaSets, StatefulSets, DaemonSets, Jobs, CronJobs, Ingress, PVC, ConfigMaps, Secrets, Namespaces, ResourceQuotas, NetworkPolicies, RBAC, CRD.

每个资源对象都有一个特定的API版本和种类（Kind），并且通常包含以下几个部分：
```
API版本（API Version）：指定使用的Kubernetes API版本，例如v1、apps/v1等。
种类（Kind）：指定资源的类型，如Pod、Service、Deployment等。
元数据（Metadata）：包含了资源的名称、命名空间、标签和其他标识信息。
规范（Spec）：定义了资源的期望状态。对于不同的资源类型，规范中的字段会有所不同。
状态（Status）：表示资源的当前状态。这是由Kubernetes系统维护的，反映了资源的实际状态。
```
资源对象通过YAML或JSON格式的文件定义 (这些文件通常被称为清单文件, manifest files)，然后使用kubectl命令行工具或API调用来创建、更新、删除和管理这些资源。

21. pod可能出现的状态
```
1) Pending
描述：Pod 已被 Kubernetes 系统接受，但有一个或多个容器尚未创建。这可能是因为系统正在等待某些资源（如容器镜像下载、调度失败、存储资源不足等）。
常见原因：镜像无法拉取、资源配额不足、调度器无法找到合适的节点等。
2) Running
描述：Pod 已经被调度到一个节点上，且所有容器都已被创建。至少有一个容器正在运行，或者正在启动或重启。
常见原因：正常操作状态。
3) Succeeded
描述：Pod 中的所有容器都已成功终止，并且不会再重启。
常见原因：通常见于执行了特定任务（如批处理作业）的 Pod，任务完成后容器自然退出。
4) Failed
描述：Pod 中的所有容器都已终止，且至少有一个容器以失败状态（非零退出状态）终止。容器不会被重启。
常见原因：应用程序错误、配置错误、权限问题等导致容器非正常退出。
5) Unknown
描述：由于某种原因，Pod 的状态无法被 Kubernetes 确定。
常见原因：通信错误导致 Kubernetes 主节点无法接收到节点上 kubelet 的更新。
6) CrashLoopBackOff
描述：这不是 Pod 的状态，而是 Pod 中一个或多个容器的状态。表示容器启动失败，尝试重启，但持续失败。
常见原因：应用程序启动失败、配置错误、依赖服务不可用等。
7) ImagePullBackOff
描述：这同样是容器的状态，表示 Kubernetes 试图从镜像仓库拉取容器镜像失败。
常见原因：镜像不存在、认证失败、网络问题等。
8) Terminating
描述：Pod 正在被删除的过程中。这个状态不是 Kubernetes 官方文档中明确列出的状态，但你会在使用 kubectl 命令时看到。
常见原因：Pod 被用户或控制器删除。
9) Evicted
描述：Pod 被系统强制终止。
常见原因：节点上的资源紧张（如磁盘空间不足），系统决定终止一些 Pod 以释放资源。
```
