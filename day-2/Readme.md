## Day 2

### Questão 1

- Precisamos levantar algumas informações sobre o nosso Cluster:
    - 1.Quantos nodes são workes?
    - 2.Quantos nodes são masters?
    - 3.Qual é o Pod Network (CNI) que estamos utilizando?
    - 4.Qual o CIDR dos pods no segundo worker?
    - 5.Qual o DNS que estamos rodando para o Cluster

### Resposta 1

    - 1.Tres nodes são Workers
    ```bash
    kubectl get nodes
    ```
    - 2.Um Node é Master
    ```bash
    kubectl get nodes
    ```
    - 3.Weave-Net
    ```bash
    kubectl get pods -n kube-system
    ```
    ```bash
    ssh NODE
    cd /etc/cni
    ls -lha
    ```
    - 4.10.32.0.0/12
```bash
kubectl describe nodes <NODE_NAME> | grep -i PodCIDR
```

```bash
kubectl get node -o jsonpath="{range .items[*]}{.metadata.name} {.spec.podCIDR}"
```

    - 5.coreDNS

### Questão 2

-

### Resposta 1



```bash

```

```bash

```

### Questão 3

-

### Resposta 1



```bash

```

```bash

```
