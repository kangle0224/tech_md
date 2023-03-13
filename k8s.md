# 1. kubectl命令

```bash
# 查看pod标签
kubectl get pods --namespace openstack --show-labels 
# 更新deployment
kubectl edit deployment.v1.apps/nginx-deployment
# 查看状态
kubectl rollout status deployment.v1.apps/nginx-deployment
# describe
kubectl describe deploy nginx-deployment
# 更新并记录
kubectl set image deployment nginx-deployment nginx=dotbalo/canary:v2 --record
# 查看更新历史
kubectl rollout history deployment/nginx-deployment
# 查看某次更新的详细信息
kubectl rollout history deployment/nginx-deployment --revision=3
# 回滚到上一个稳定版本
kubectl rollout undo deployment/nginx-deployment
# 回滚到指定版本
kubectl rollout undo deployment/nginx-deployment --to-revision=2
# 扩容
kubectl scale deployment.v1.apps/nginx-deployment --replicas=5
# 暂停
kubectl rollout pause deployment/nginx-deployment
# 恢复
kubectl rollout resume deployment.v1.apps/nginx-deployment
# 设置revision数量
.spec.revisionHistoryLimit
  当为0时，不保留历史记录






```

# 2. statefulset

```bash
常用于部署有状态的且需要有序启动的应用
# 扩容
kubectl scale sts web --replicas=5
# 缩容
kubectl patch sts web -p '{"spec": {"replicas": 3}}'
# 查看滚动更新的状态
kubectl rollout status sts/web
# 分段更新
kubectl patch statefulset web -p '{"spec":{"updateStrategy":{"type":"RollingUpdate","rollingUpdate":{"partition":3}}}}'
# 删除sts
# 非级联删除，不会删除pod
kubectl delete sts xxx --cascade=false
# 级联删除，会删除pod
kubectl delete sts xxx 
```

# 3. daemonset

```bash
它在符合匹配条件的节点上部署一个pod


```

# 4. label

```bash
kubectl label node k8s-node-2 reigon=subnet2
kubectl get no -l region=subnet7
# 查看目前已有的标签
kubectl get service --show-labels
# 选择app为productpage或reviews但不包括version=v1的service
kubectl get svc -l version!=v1,'app in (details,productpage)' --show-labels
# 选择label的key为app的service
kubectl get svc -l app --show-labels
# 修改标签(将version=v1改为v2)
kubectl label svc canary-v1 -n canary-production version=v2 --overwrite
# 删除标签
kubectl label svc canary-v1 -n canary-production version-
```

# 5. service

```bash
service主要用于pod直接的同学，对于pod的ip地址而言，service是提前定义好并且不变的资源类型

```
