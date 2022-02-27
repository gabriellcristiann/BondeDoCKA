## Day 2

### Questão 1

Precisamos levantar algumas informações sobre o nosso Cluster:

- 1.Quantos nodes são workes?
- 2.Quantos nodes são masters?
- 3.Qual é o Pod Network (CNI) que estamos utilizando?
- 4.Qual o CIDR dos pods no segundo worker?
- 5.Qual o DNS que estamos rodando para o Cluster

### Respostas

- 1.Quantos nodes são workes?

    R- Tres nodes são Workers

```bash
kubectl get nodes
```

- 2.Quantos nodes são masters?

    R- Um Node é Master

```bash
kubectl get nodes
 ```

- 3.Qual é o Pod Network (CNI) que estamos utilizando?

    R- Weave-Net

```bash
kubectl get pods -n kube-system
```

```bash
ssh NODE
cd /etc/cni
ls -lha
```

- 4.Qual o CIDR dos pods no segundo worker?

    R- 10.32.0.0/12

```bash
kubectl describe nodes <NODE_NAME> | grep -i PodCIDR
```

```bash
kubectl get node -o jsonpath="{range .items[*]}{.metadata.name} {.spec.podCIDR}"
```

- 5.Qual o DNS que estamos rodando para o Cluster

    R- coredns

```bash
kubectl get pods -n kube-system
```

### Questão 2

Precisamos criar um Pod com as seguintes caracteristicas:

- Precisa ter um container rodando a imagem do Nginx, com um volume montado no diretório html do nginx
- Precisa ter um outro container rodando busybox e adicionando algum conteudo ao arquivo /tmp/index.html

```bash
    - => command: ["sh", "-c", "while true; do uname -a >> /tmp/index.html; date >> /tmp/index.html; sleep 2; done"]
```

- Precisamos ter um outro container rodando o busybox e execultando o seguinte comando:

```bash
    - => command: ["sh", "-c", "tail -f /tmp/index.html"]
```

### Respostas

Criamos o arquivo pod.yaml e nele adicionamos os tres containers

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: meu-pod
spec:
  containers:
    - name: container-1
      image: nginx
      ports:
        - containerPort: 80
      volumeMounts:
        - name: workdir
          mountPath: /usr/share/nginx/html
      resources:
        limits:
          memory: '1Gi'
          cpu: '800m'
        requests:
          memory: '700Mi'
          cpu: '400m'

    - name: container-2
      image: busybox
      command: ["sh", "-c", "while true; do uname -a >> /tmp/index.html; date >> /tmp/index.html; sleep 2; done"]
      volumeMounts:
        - mountPath: /tmp/
          name: workdir
      resources: {}

    - name: container-3
      image: busybox
      command: ["sh", "-c", "tail -f /tmp/index.html"]
      volumeMounts:
        - mountPath: /tmp/
          name: workdir
      resources: {}
          
  dnsPolicy: Default
  volumes:
    - name: workdir
      emptyDir: {}
```

Criando o Pod

```bash
kubectl create -f pod yaml
```
Verificando os logs para ver o funcionamento

```bash
kubectl logs -f meu-pod container-1
kubectl logs -f meu-pod container-2
kubectl logs -f meu-pod container-3
kubectl exec -ti meu-pod -c container-1 --bash
```

### Questão 3

-

### Resposta 1



```bash

```

```bash

```
