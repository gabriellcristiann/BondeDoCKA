## DAY 1

### Questão 1

Criar um pod utilizando a imagem do Nginx 1.18.0, com o nome de giropops no namespace strigus.

### Resposta 1

Nesse caso temos duas formas.
A primeira, utilizando somente a linha de comando:

```bash
kubectl run giropops --image nginx:1.18.0 --port 80 --namespace strigus
```

A segunda e a mais recomendada. Eu acho ela recomendada pelo fato de você poder analizar com mais tranquilidade o que você esta criando, mas é a minha opinião apenas. =)

```bash
kubectl run giropops --image nginx:1.18.0 --port 80 --namespace strigus --dry-run=client -o yaml > pod.yaml

kubectl create -f pod.yaml 
```

### Questão 2

Aumentar a quantidade de replicas do deployment girus, que está utilizando a imagem do nginx 1.18.0, para 3 replicas. O deployment está no namespace strigus.

### Resposta 1

```bash
kubectl scale deployment girus --replicas 3
```

```bash
kubectl edit deployment girus # dentro do arquivo alteramos a quantidade de replicas e saimos
```

```bash
kubectl create deployment --namespace strigus girus --port 80 --image nginx:1.18.0 --replicas 3 --dry-run=client -o yaml > deployment.yaml

kubectl create -f deployment.yaml
```

### Questão 3

Precisamos atualizar a versão di Nginx do Pod giropops. Ele está na versão 1.18.0 e precisamos atualizar para versão 1.21.1

### Resposta 1

```bash
kubectl edit pod giropops # Lá mudamos a versão do Nginx
```

```bash
kubectl set image pod giropops web=nginx:1.21.1 # Onde web é o nome do nosso container
```

```bash
kubectl get pods giropops -o yaml > pod4.yaml # Ao abrir o yaml, editamos-o removendo "lixo" e alterando a imagem

kubectl apply -f pod4.yaml
```

```bash

```