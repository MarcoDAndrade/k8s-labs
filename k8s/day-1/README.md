# Primeiro dia o recomeço!

Estes sao os comandos principais para executar dentro da maquina "master" criada para seu lab!


## Conceitos

Começa entendendo sobre o que estamos falando [aqui](https://livro.descomplicandokubernetes.com.br/pt/day_one/#o-que-iremos-ver-hoje)!



## Mão na massa

Indo direto aos pontos, para meu próprio lab, vou salvando os comandos:

```shell
curl -s -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl

chmod +x ./kubectl

sudo mv ./kubectl /usr/local/bin/kubectl

kubectl version --client
```

### Facilitando a vida com "autocomplete":

Eu não gostei do que o Jefferson fez no tutorial, e fui procurar a [documentação](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#optional-kubectl-configurations-and-plugins), e para minha surpresa... foi muito proximo do que ele concluiu :D

```shell
echo "source <(kubectl completion bash)" >> ~/.bashrc
```

### Alias

Para facilitar a vida:

```shell
echo 'alias k=kubectl' >>~/.bashrc
echo 'complete -o default -F __start_kubectl k' >>~/.bashrc
```

### Carregando tudo em memoria

Este comando serve para carregar as variaveis, sem precisar abrir uma nova sessão.

```shell
source ~/.bashrc
```
## Criando o Cluster Kubernetes

Chego aqui com a proposta de criar por mais de uma forma...


### Criando com o KIND

O Kind (Kubernetes in Docker) é outra alternativa para executar o Kubernetes num ambiente local para testes e aprendizado, mas não é recomendado para uso em produção.

```shell
curl -s -Lo ./kind https://kind.sigs.k8s.io/dl/v0.14.0/kind-linux-amd64

chmod +x ./kind

sudo mv ./kind /usr/local/bin/kind
```


#### Instalando o Docker localmente

Aqui baixamos o Docker e ajustamos nosso usuario para ter permissão de trabalho, sendo necessário um novo login, ou rodar fazer um loop, abrindo nova sessão, para ter a permissão!

```shell
curl https://get.docker.com | sudo bash

sudo usermod -G docker vagrant

exec sudo su - vagrant
```

#### Criando um cluster simples

```shell
$ kind create cluster --name giropops

Creating cluster "giropops" ...
 ✓ Ensuring node image (kindest/node:v1.24.0) 🖼
 ✓ Preparing nodes 📦  
 ✓ Writing configuration 📜 
 ✓ Starting control-plane 🕹️ 
 ✓ Installing CNI 🔌 
 ✓ Installing StorageClass 💾 
Set kubectl context to "kind-giropops"
You can now use your cluster with:

kubectl cluster-info --context kind-giropops

Thanks for using kind! 😊

```

Para visualizar os seus clusters utilizando o kind, execute o comando a seguir.

```shell
kind get clusters
  Liste os nodes do cluster.

kubectl get nodes
```

Exercicio feito, cleanup é parte do exercício!

```shell
kind delete clusters $(kind get clusters)
```

#### Criando um cluster multi-nodes

Agora a brincadeira aumenta.

Antes de seguir este passo, tenha certeza que entendeu o os componentes do K8s:

- Control-plane
- Workers

O material de referencia vai te ajudar, se tem dúvidas, revise.


```shell
cat << EOF > $HOME/kind-3nodes.yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
  - role: control-plane
  - role: worker
  - role: worker
EOF


# Comando
kind create cluster --name kind-multinodes --config $HOME/kind-3nodes.yaml
```


Agora a brincadeira com os comandos vai fazer mais sentido:

- Verificando os **nodes** criados:

    ```shell
    kubectl get nodes
    ```

    E se você também ficou curioso com estes "nodes"... eis a resposta:

    ```code
    docker ps
    CONTAINER ID   IMAGE                  COMMAND                  CREATED         STATUS         PORTS                       NAMES
    e109908b7599   kindest/node:v1.24.0   "/usr/local/bin/entr…"   6 minutes ago   Up 6 minutes   127.0.0.1:35563->6443/tcp   kind-multinodes-control-plane
    539553f6353e   kindest/node:v1.24.0   "/usr/local/bin/entr…"   6 minutes ago   Up 6 minutes                               kind-multinodes-worker
    ee79e3f6e94a   kindest/node:v1.24.0   "/usr/local/bin/entr…"   6 minutes ago   Up 6 minutes                               kind-multinodes-worker2
    ```

- Verificar os namespaces existentes

    ```shell
    kubectl get namespaces
    ```

- Verificar os pods existentes:

    ```shell
    $ kubectl get pod -n kube-system -A -o wide
    NAMESPACE            NAME                                                    READY   STATUS    RESTARTS   AGE     IP           NODE                            NOMINATED NODE   READINESS GATES
    kube-system          coredns-6d4b75cb6d-g99lk                                1/1     Running   0          7m44s   10.244.0.3   kind-multinodes-control-plane   <none>           <none>
    kube-system          coredns-6d4b75cb6d-j5qp2                                1/1     Running   0          7m44s   10.244.0.2   kind-multinodes-control-plane   <none>           <none>
    kube-system          etcd-kind-multinodes-control-plane                      1/1     Running   0          7m56s   172.18.0.3   kind-multinodes-control-plane   <none>           <none>
    kube-system          kindnet-5pt8z                                           1/1     Running   0          7m45s   172.18.0.3   kind-multinodes-control-plane   <none>           <none>
    kube-system          kindnet-n9pt5                                           1/1     Running   0          7m21s   172.18.0.4   kind-multinodes-worker          <none>           <none>
    kube-system          kindnet-rnnnh                                           1/1     Running   0          7m20s   172.18.0.2   kind-multinodes-worker2         <none>           <none>
    kube-system          kube-apiserver-kind-multinodes-control-plane            1/1     Running   0          7m56s   172.18.0.3   kind-multinodes-control-plane   <none>           <none>
    kube-system          kube-controller-manager-kind-multinodes-control-plane   1/1     Running   0          7m56s   172.18.0.3   kind-multinodes-control-plane   <none>           <none>
    kube-system          kube-proxy-cv9fh                                        1/1     Running   0          7m21s   172.18.0.4   kind-multinodes-worker          <none>           <none>
    kube-system          kube-proxy-j7qvq                                        1/1     Running   0          7m20s   172.18.0.2   kind-multinodes-worker2         <none>           <none>
    kube-system          kube-proxy-ljxp4                                        1/1     Running   0          7m45s   172.18.0.3   kind-multinodes-control-plane   <none>           <none>
    kube-system          kube-scheduler-kind-multinodes-control-plane            1/1     Running   0          7m56s   172.18.0.3   kind-multinodes-control-plane   <none>           <none>
    local-path-storage   local-path-provisioner-9cd9bd544-z8vth                  1/1     Running   0          7m44s   10.244.0.4   kind-multinodes-control-plane   <none>           <none>
    ```

    Dando a minha interpretação durante o estudo:
    
    - Control plane está dentro de kube-system

      - etcd (etcd-kind-multinodes-control-plane)- base de estado do ambiente
      - apiserver - interface API para controlar tudo
      - controller-manager - o nome já descreve :D
      - scheduler - o nome já diz tudo

      Os itens não fazem parte do Control Plane, mas estarão presentes em todos os nodes:

      - coredns - para resolver nomes
      - kindnet - para fazer a gestão da rede, nesta abordagem de cluster
      - proxy - para fazer a comunicação externa com seus pods



## Avançando rapido

Vou pular os conceitos iniciais sobre POD, pois não quero refazer o material, apenas registrar meus avanços ;)


### Criando um template


```shell
# Gerando template simples
kubectl run meu-nginx --image nginx --port 80 --dry-run=client -o yaml > pod-template.yaml

# Aplicando
kubectl create -f pod-template.yaml

# Consultando
kubectl get pods
```

```shell
# cleanup
kubectl delete -f pod-template.yaml
kubectl delete service nginx
```