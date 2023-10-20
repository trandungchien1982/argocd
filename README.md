# argocd
- Series về học ArgoCD/ Demo thực tế
- Các ví dụ liên quan đến ArgoCD từ cơ bản đến nâng cao<br/>

# Môi trường phát triển
- Ubuntu 20.04 LTS
- K8S được xây dựng dựa trên K3S + K3D (cluster có 1 node)
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

==============================================================

# Ví dụ [03.UsingHelm]
==============================================================


**Tiến hành deploy apps Java + k8s sử dụng Helm bằng ArgoCD:**
<br/>(Hiện tại đang bị stuck ở bước tạo App mới trên ArgoCD - Sẽ quay lại chi tiết sau ...)
- https://github.com/trandungchien1982/k8s/tree/08.ShowAllEnvs%2BHelm
- Bản thân New Apps khi tạo ra thì ta có thể điều chỉnh file YAML và tiến hành customize New Apps dựa vào file YAML config.
```shell
ArgoCD-NewApp-Using-Helm.yaml
```

**Kiểm tra toàn bộ thông tin K8S dùng K9S CLI:**
```shell
  k9s
  ----------------------------------------------------------------------------------------------
  Context: k3d-k3s-dev                              <0> all       <a>      Attach     <l>       Logs               <y> YAML                                          ____  __.________        
  Cluster: k3d-k3s-dev                              <1> default   <ctrl-d> Delete     <p>       Logs Previous                                                       |    |/ _/   __   \______ 
  User:    admin@k3d-k3s-dev                                      <d>      Describe   <shift-f> Port-Forward                                                        |      < \____    /  ___/ 
  K9s Rev: v0.27.4                                                <e>      Edit       <s>       Shell                                                               |    |  \   /    /\___ \  
  K8s Rev: v1.23.6+k3s1                                           <?>      Help       <n>       Show Node                                                           |____|__ \ /____//____  > 
  CPU:     1%                                                     <ctrl-k> Kill       <f>       Show PortForward                                                            \/            \/  
  MEM:     1%                                                                                                                                                                                 
┌────────────────────────────────────────────────────────────────────────────────────── Pods(all)[17] ──────────────────────────────────────────────────────────────────────────────────────┐
│ NAMESPACE↑      NAME                                                PF  READY   RESTARTS STATUS       CPU  MEM  %CPU/R  %CPU/L  %MEM/R  %MEM/L IP           NODE                   AGE    │
│ argocd          argocd-application-controller-0                     ●   1/1            4 Running        9   70     n/a     n/a     n/a     n/a 10.42.1.56   k3d-k3s-dev-agent-0    37h    │
│ argocd          argocd-applicationset-controller-6b5b54d544-4h8mh   ●   1/1            4 Running        1   19     n/a     n/a     n/a     n/a 10.42.1.51   k3d-k3s-dev-agent-0    37h    │
│ argocd          argocd-dex-server-7656cd55f4-jjh4p                  ●   1/1            6 Running        1   16     n/a     n/a     n/a     n/a 10.42.0.46   k3d-k3s-dev-server-0   37h    │
│ argocd          argocd-notifications-controller-746457787-zjlht     ●   1/1            4 Running        1   18     n/a     n/a     n/a     n/a 10.42.1.52   k3d-k3s-dev-agent-0    37h    │
│ argocd          argocd-redis-774dbf45f8-57v2f                       ●   1/1            4 Running        2    3     n/a     n/a     n/a     n/a 10.42.0.42   k3d-k3s-dev-server-0   37h    │
│ argocd          argocd-repo-server-76d7968f94-r75wx                 ●   1/1            4 Running        1   41     n/a     n/a     n/a     n/a 10.42.0.40   k3d-k3s-dev-server-0   37h    │
│ argocd          argocd-server-5db7487c98-qszpj                      ●   1/1            4 Running        1   27     n/a     n/a     n/a     n/a 10.42.1.49   k3d-k3s-dev-agent-0    37h    │
└───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
```

**Truy cập vào các trang tương ứng:**
```shell
  http://localhost:7100/show-env-helm
```
