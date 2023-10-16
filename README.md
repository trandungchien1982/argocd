# argocd
- Series về học ArgoCD/ Demo thực tế
- Các ví dụ liên quan đến ArgoCD từ cơ bản đến nâng cao<br/>

# Môi trường phát triển
- K8S được xây dựng dựa trên K3S + K3D (cluster chỉ có 1 node)
  ```shell
  k3d cluster create k3s-dev --port 7100:80@loadbalancer --port 7143:443@loadbalancer --api-port 6443 --servers 1 --agents 1 --network common-network --token K3D_K3S_CHIEN_TOKEN
  ```
- Tham khảo cách xây dựng Cluster K8S:
  ```shell
  https://github.com/trandungchien1982/k8s/tree/01.HelloWorld
  ```
  
# Folder liên quan trên Windows
```
D:\Projects\argocd
```
