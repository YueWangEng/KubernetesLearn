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
   2) 2) kubectl get pods —all-namespaces   

5. Create a service redis-service to expose the redis application within the cluster on port 6379.   
kubectl expose pod redis --port=6379 --name=redis-service   

6. 创建对象   
   1）不使用yaml file创建object的时候，只有pod才能用run(且无需只能关键字pod)，别的例如replicates, deployment，需使用create(且需指明关键字)   
   kubectl create deployment webapp --image=kodekloud/webapp-color --replicas=3   
   2）但使用yaml file的时候，都用create，且无需只能关键字pod
   kubectl create –f pod-definition.yml）  

8. kubectl create deployment redis-deploy --namespace=dev-ns  --image=redis --replicas=2   
按照yaml中的顺序，image放在namespace之后   

9. Create a pod called httpd using the image httpd:alpine in the default namespace. Next, create a service of type ClusterIP by the same name (httpd). The target port for the service should be 80.   
kubectl run httpd --image=httpd:alpine --port=80 --expose=true   

10. 检查有没有scheduler   
kubectl get pod -n kube system   

11. 更新yaml以后，重新生成pod，或者将pod从一个node转移到别的node。
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
    方法之一： Kubectl replace --force -f (yaml)
