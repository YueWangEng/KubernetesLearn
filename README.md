# KubernetesLearn

1. 在生成pod时，显示yaml
kubectl run redis --image redis123 --dry-run=client -o yaml
写入具体文件中
kubectl run redis --image redis123 --dry-run=client -o yaml > pod-redis.yaml

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

6. 不使用yaml file创建object的时候，只有pod才能用run(且无需只能关键字pod)，别的例如replicates, deployment，需使用create(且需指明关键字）
kubectl create deployment webapp --image=kodekloud/webapp-color --replicas=3

但使用yaml file的时候，都用create，且无需只能关键字pod
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
kubectl get pods --selector <items1>,<items2>,… (逗号之间不可有空格）

12. 显示所有内容
kubectl get all

13. 小tip
显示内容时，不限表头，可用例如，kubectl get pods --selector <items> —no headers
只显示行数，kubectl get pods --selector <items> —no headers | wc -l
