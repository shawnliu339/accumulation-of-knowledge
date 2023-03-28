# Kubernetes学习笔记
## 几个常见概念
cluster, node, pod, 抽象概念service  
强烈推荐阅读官方文档，`教程/学习Kubernetes基础知识/` 下的内容，有图所以非常易于理解。

简单总结自己的理解。cluster是集群，上面运行着node，node上运行着pod，pod是kubernete里的基础单元。  
pod上运行容器，可以是容器化的应用，也可以是volume(增加容量)的容器。  
service是抽象概念，可以等同于app，它可以由多个pod组成，每个pod又可以分布在不同的node上。


## 常用command
1. 查看pod详情
```
kubectl get pods -o wide
```
2. 一览namespace下的所有pod
```
kubectl get pods --namespace= -o wide
```
3. namepace一览
```
kubectl get namespace
```
4. cluster一览
```
kubectl config get-clusters
kubectl config get-contexts
```
5. 切换cluster
```
kubectl config use-context your-cluster-name
```
6. 打开dashboard
```
open http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/workloads?namespace=default
open $(vksctl addon kubernetes-dashboard url)
```
7. 进入Server
```
kubectl get pods -n <namespace>
kubectl exec -it -n <namespace> <pod> -- /bin/bash
```
8. 复制k8s container上的文件
```
kubectl cp namespace/pod_name:src dest
```
没有tar的情况
```
kubectl exec -n namespace pod_name -- cat src/file  > dest/file
```
9. 追加调试pod
https://kubernetes.io/zh-cn/docs/tasks/debug/debug-application/debug-running-pod/#ephemeral-container-example
```
kubectl run test --image=image:tag --restart=Never -n namespace --command -- sleep 3600
```

10. 


## Reference
1. 下面的文章讲解了kubernets和container之间的关系。
https://blog.csdn.net/yanghaolong/article/details/86680282
自己的理解：docker是运行容器的程序/环境(container runtime)，kubernetes是管理容器的程序