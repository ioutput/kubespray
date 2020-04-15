
# kebuspray安装
* 确保master系统里安装有python3.6及pip
* 安装sudo pip3 install -r requirements.txt
* ssh-keygen生成免密通信密钥，ssh-copy-id root@192.168.5.55分发到各个node节点
* https://github.com/kubernetes-sigs/kubespray下载到master并复制inventory/sample别名并修改hosts.ini配置所有服务器ip
* 国内的一些k8s要翻墙，修改roles/download/defaults/main.yml设置gcr_image_repo，quay_image_repo=registry.cn-hangzhou.aliyuncs.com/google_containers；kube_image_repo=ccr.ccs.tencentyun.com/easyk8s；修改inventory/sample/group_vars/k8s-cluster/k8s-cluster.yml中kube_image_repo=ccr.ccs.tencentyun.com/easyk8s
* 在目录里执行命令ansible-playbook -i inventory/mycluster/hosts.yaml  --become --become-user=root cluster.yml；若提示etcd问题则是版本不同造成，修改roles/etcd/tasks/configure.yml里的etcdctl命令并注释
* 安装公共文件服务：sudo apt-get install nfs-kernel-server;各节点也要安装apt-get install nfs-common才可访问
* kubectl create -f xxx.yml安装pod
* kubectl get svc查看服务端口
* https://github.com/kubernetes/dashboard可视化界面安装；
  - 启动可视化服务 nohup kubectl port-forward --namespace kubernetes-dashboard service/kubernetes-dashboard 10443:443 --address 0.0.0.0 &
  - 添加用户 kubectl create serviceaccount dashboard-admin -n kube-system
  - 获取token:kubectl describe secret -n kube-system dashboard-admin
