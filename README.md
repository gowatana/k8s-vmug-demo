# VMUG 2020/04/24 Docker & k8s Demo。

# ただのDocker。

Dockerコンテナの起動。

```
# docker run -itd -p 8000:80 --rm nginx
# docker run -it --rm vmware/powerclicore
```

```
Connect-VIServer 192.168.10.11 -Force -User administrator@vsphere.local -Password VMware1!
```


# K8s の接続情報、構成

接続情報。

```
# kubectl config view
```

K8sクラスタ。

- -o wide でIP見る → vSphere Client (192.168.10.11)
- Vm = コンテナホスト（get node）

```
# kubectl get nodes
# kubectl get nodes -o wide
```

config 読み込み

```
# export KUBECONFIG=/root/vsphere-quickstart.kubeconfig
```

# PV なし

K8sでのコンテナ起動。

StatefulSet 作成

```
# cat k8s-pv-demo/test-ss.yml
# kubectl apply -f k8s-pv-demo/test-ss.yml
```

# cat k8s-pv-demo/test-ss.yml

Pod見る

コンテンツを配置。

```
# kubectl cp index.html nginx-0:/usr/share/nginx/html/index.html
```

ブラウザからWeb確認。

Pod消す

```
# kubectl delete pod nginx-0
```

→ Pod 自動復活。コンテンツは揮発。

# PVあり

vSphere Client からCNSコンテナボリューム確認。
VMから見ても変わらない。

```
# kubectl cp index.html nginx-0:/usr/share/nginx/html/index.html
```

PVC見る
PV見る

コンテンツ配置

```
# index.html
# kubectl cp index.html nginx-0:/usr/share/nginx/html/index.html
```

Pod消す

```
# kubectl delete pod nginx-0
```

→ Pod 自動復活。コンテンツも残る。（PVにある）

コンテナ内では、/usr/share/nginx/html/ マウントあり

```
# kubectl exec -it nginx-0 bash
# df -h
```

PVの実体を、vSphere Clientから確認。

- CNS画面から、VM, vSAN
- VM - VMDK


# Cluster API

- WIP...


以上

