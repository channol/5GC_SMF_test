# 5GC_SMF_test
5GC_SMF_test handbook
# k8s环境
ip 172.0.5.156 

# k8s设备
172.0.5.156 master  
172.0.5.157 slave  
172.0.5.158 slave  

# 命令
`kubectl get pod --all-namespaces -owide  `  
`kubectl get pod --all-namespaces -owide | grep casa  `  
`kubectl get pod --all-namespaces -owide | grep pcf  `  
`kubectl get pod --all-namespaces -owide | grep chf `  

在两个node都要开启/opt/wireshark/bin/wireshark 抓cni0/cni1接口  
`/opt/wireshark/bin/wireshark`

在master：

# 进入gnb  

`kubectl -n casa exec -it casa-gnb-8697d879df-26clb sh`

# 启动gnb

`sim-gnb -a 10.244.2.3 -p 31295 -h 38412 -B 65501 -n 1 -u 1 -G 0 --imsi 45005100000000`

# 查看ip端口  

`kubectl get svc --all-namespaces`

# 查看log

`kubectl logs -n <namespace> pod_name`

# 查看etcd参数

`ETCDCTL_API=3 etcdctl  --endpoints=10.244.2.11:2379  get / --prefix`

# Python virtual environment

`sudo apt-get install python3-venv  `  
`python3 -m venv ~/venvs/5gc-lab  `  
`. ~/venvs/5gc-lab/bin/activate  `  
`pip install --upgrade pip  `  
`pip install click_shell  `  

# Install Kubectl

`curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl  `  
`chmod +x ./kubectl  `  
`mv ./kubectl /usr/local/bin/kubectl  `  
`kubectl version  `

# Configure Kubectl
Substitute example with your namespace, for example "amf"  

`export NAMESPACE=example `  
`kubectl config set-cluster 5gc --server=https://10.180.70.41:6443 --insecure-skip-tls-verify=true  `  
`kubectl config set-credentials admin@5gc --token=eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJkZWZhdWx0Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZWNyZXQubmFtZSI6ImNsdXN0ZXItYWRtaW4tZGFzaGJvYXJkLXNhLXRva2VuLXE3ODd6Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImNsdXN0ZXItYWRtaW4tZGFzaGJvYXJkLXNhIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQudWlkIjoiNTlmNmNhY2UtMTA3NS0xMWU5LWJmMTctZmExNjNlNmY5Mzg2Iiwic3ViIjoic3lzdGVtOnNlcnZpY2VhY2NvdW50OmRlZmF1bHQ6Y2x1c3Rlci1hZG1pbi1kYXNoYm9hcmQtc2EifQ.UV3v4XBnYGJ_ogHQDIEyBDFSr-nW9BxICJzI3FUaMMBl-1KhYtPD8muVZnG8zkX5K1fVgX0fY1OaVAL2_O0432eyjyj6_hMdXelc3_gEyvjexFDTNwoycTgAYAcEKMFA7edLf9ejqbl6v0rTsUh4BPC8_e4A6R7_Dq14qYcPALlqvbQ5shZwMAQ4u7Z-jiOjCYcM4CiqyF7YCeoySL7xsOiE_DJqGpOlubqjRPKn5G8P03vjiKwlYlX_8yPrEC7WYP0zszfccL39vy1_51QUwPgW2ANwMebBMhPyTiiAa8Yy8ol-Mg1r4alX0gX6sMhxT_sv1at0A_FIWKyc8n90OQ  `  
`kubectl config set-context 5gc --user=admin@5gc --cluster=5gc --namespace=$NAMESPACE  `  
`kubectl config use-context 5gc  `  
`kubectl cluster-info  `  


# Install Helm   

`wget https://storage.googleapis.com/kubernetes-helm/helm-v2.11.0-linux-amd64.tar.gz  `  
`tar -xvf helm-v2.11.0-linux-amd64.tar.gz  `  
`mv ./linux-amd64/helm /usr/local/bin/helm  `  
`helm init --client-only  `  
`helm version `

# Add Casa Helm Repository  

`helm repo add casa http://10.180.180.30:8083  `  
`helm repo update `

# 实例ip
```
root@master:~# kubectl get pod --all-namespaces -owide|  grep casa
casa             casa-amf-agent-64567fc488-csh9b                 1/1     Running   0          3h17m   10.244.2.4     node2    <none>
casa             casa-ausf-5bcf9f56d4-5rgjp                      1/1     Running   0          3h17m   10.244.2.10    node2    <none>
casa             casa-etcd-0                                     1/1     Running   0          3h17m   10.244.2.11    node2    <none>
casa             casa-gnb-8697d879df-26clb                       1/1     Running   0          3h17m   10.244.2.12    node2    <none>
casa             casa-mgmt-service-7cd6464dd7-thhfr              1/1     Running   0          3h17m   10.244.2.9     node2    <none>
casa             casa-n2mgr-697bd5bf4c-rjspd                     1/1     Running   0          3h17m   10.244.2.3     node2    <none>
casa             casa-nrepo-5f7565fcfd-fskpr                     1/1     Running   0          3h17m   10.244.2.6     node2    <none>
casa             casa-nrepo-timer-56c456c678-kgzkl               1/1     Running   0          3h17m   10.244.2.8     node2    <none>
casa             casa-nssel-7956f5f95-ssmz8                      1/1     Running   0          3h17m   10.244.2.7     node2    <none>
casa             casa-sm-5d876979c6-v6ttl                        1/1     Running   0          3h14m   10.244.1.139   node1    <none>
casa             casa-smf1-agent-5bcc85ff74-nbdql                1/1     Running   0          3h14m   10.244.1.140   node1    <none>
casa             casa-udm-588c5575c9-mkxh9                       1/1     Running   0          3h17m   10.244.1.131   node1    <none>
casa             casa-udm-agent-7f79dd8bb6-w49np                 1/1     Running   2          3h17m   10.244.1.134   node1    <none>
casa             casa-uemgr-58584fbb88-hfqqk                     1/1     Running   0          3h14m   10.244.2.17    node2    <none>
casa             casa-upf1-agent-599484b48b-ffcbg                1/1     Running   0          3h14m   10.244.1.141   node1    <none>
casa             redis-master-0                                  1/1     Running   0          3h17m   10.244.2.13    node2    <none>
casa             rediscluster-casa-fbh9r                         1/1     Running   0          3h16m   10.244.1.137   node1    <none>
casa             rediscluster-casa-gnl9h                         1/1     Running   0          3h17m   10.244.2.14    node2    <none>
casa             rediscluster-casa-hkzwh                         1/1     Running   0          3h16m   10.244.2.15    node2    <none>
casa             rediscluster-casa-hz7pk                         1/1     Running   0          3h16m   10.244.1.138   node1    <none>
casa             rediscluster-casa-jbt4f                         1/1     Running   0          3h17m   10.244.1.136   node1    <none>
casa             rediscluster-casa-vflnf                         1/1     Running   0          3h15m   10.244.2.16    node2    <none>
root@master:~# kubectl get pod --all-namespaces -owide|  grep pcf
pcf              am-policies-546db5dbb4-b55bz                    4/4     Running   0          3h32m   10.244.2.250   node2    <none>
pcf              decision-engine-6b5dddfcc6-nzh99                2/2     Running   0          3h32m   10.244.2.251   node2    <none>
pcf              mock-amf-6c5c5f57d-rppxn                        1/1     Running   0          3h32m   10.244.1.112   node1    <none>
pcf              openet-pcf-ambassador-5769fbd7fc-25np9          2/2     Running   0          3h32m   10.244.1.111   node1    <none>
pcf              openet-pcf-perf-test-udr-6cdd4c4b97-b2rck       2/2     Running   0          3h32m   10.244.1.113   node1    <none>
pcf              pcf-configuration-server-79f6c4d7b4-9dpm9       2/2     Running   0          3h32m   10.244.2.252   node2    <none>
pcf              pcf-gui-8dc8bd5d9-qz2lf                         1/1     Running   0          3h32m   10.244.2.253   node2    <none>
pcf              registration-agent-59dff5cdc4-gwc48             1/1     Running   0          3h32m   10.244.1.117   node1    <none>
pcf              sm-policies-7596bfc486-fxhf5                    4/4     Running   0          3h32m   10.244.2.254   node2    <none>
pcf              smvoltdb-0                                      1/1     Running   0          3h32m   10.244.1.116   node1    <none>
pcf              smvoltdb-1                                      1/1     Running   0          3h32m   10.244.1.118   node1    <none>
pcf              smvoltdb-2                                      1/1     Running   0          3h31m   10.244.1.119   node1    <none>
pcf              udsf-85d76fc4d7-rxkp5                           2/2     Running   0          3h32m   10.244.1.115   node1    <none>
pcfhttp          pcfhttp-ambassador-89787bf66-sb6bz              2/2     Running   0          3h32m   10.244.1.114   node1    <none>
root@master:~# kubectl get pod --all-namespaces -owide|  grep chf
chf              chf-8675db478f-l27dg                            4/4     Running   0          3h30m   10.244.1.122   node1    <none>
chf              openet-chf-ambassador-5db9b57659-nbkb6          2/2     Running   0          3h30m   10.244.1.120   node1    <none>
chf              pmvoltdb-0                                      1/1     Running   0          3h30m   10.244.1.124   node1    <none>
chf              pmvoltdb-1                                      1/1     Running   0          3h30m   10.244.1.127   node1    <none>
chf              pmvoltdb-2                                      1/1     Running   0          3h30m   10.244.1.129   node1    <none>
chf              profilemanager-576d9c956c-cmm46                 2/2     Running   0          3h30m   10.244.2.2     node2    <none>
chf              registration-agent-758dbff9cb-htjlm             1/1     Running   0          3h30m   10.244.1.125   node1    <none>
chf              smvoltdb-0                                      1/1     Running   0          3h30m   10.244.1.126   node1    <none>
chf              smvoltdb-1                                      1/1     Running   0          3h30m   10.244.1.128   node1    <none>
chf              smvoltdb-2                                      1/1     Running   0          3h30m   10.244.1.130   node1    <none>
chf              udsf-bf6f4bc9b-k9z7f                            2/2     Running   0          3h30m   10.244.1.123   node1    <none>
chfhttp          chfhttp-ambassador-f86d87856-9rdj6              2/2     Running   0          3h30m   10.244.1.121   node1    <none>
```
