### Util links

https://kubernetes.io/blog/2020/05/21/wsl-docker-kubernetes-on-the-windows-desktop/    

https://kind.sigs.k8s.io/docs/user/using-wsl2/

https://kubernetes.io/docs/reference/kubectl/cheatsheet/

## Create cluster with three nodes
    
```yml
# Delete the existing cluster
kind delete cluster --name aamvcluster
# Create a config file for a 3 nodes cluster
cat << EOF > aamvcluster-config.yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
  - role: control-plane
    extraPortMappings:
    - containerPort: 30000
      hostPort: 30000
      protocol: TCP
  - role: worker
  - role: worker
EOF
# Create a new cluster with the config file
kind create cluster --name aamvcluster --config ./aamvcluster-config.yaml
# Check how many nodes it created
kubectl get nodes
```

para probarlo:

+ kubectl create deployment nginx --image=nginx --port=80
+ kubectl create service nodeport nginx --tcp=80:80 --node-port=30000
+ curl localhost:30000