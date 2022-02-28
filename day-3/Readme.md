## Day 3

### Questão 1

Criar um Pod estatico utilizando a imagem do Nginx.

### Respostas

Para criar um pod estatico, você preicsa adicionar o manifesto de criação do pod desejado, dentro do diretório /etc/kubernetes/manifests, conforme abaixo:

```bash
cd /etc/kubernetes/manifests
kubectl run giropops --image nginx -o yaml --dry-run=client > meu-pod-estatico.yaml
vim meu-pod-estatico.yaml
```

Adicione o conteúdo abaixo:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: giropops
  labels:
    run: giropops
spec:
  containers:
  - name: web
    image: nginx
    ports:
    - name: web
      containerPort: 80
      protocol: TCP
    resources: {}    
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
 ```

### Questão 2

O nosso gerente esta assustado, pois conversando com o gerente de outra empresa, ficou sabendo que aconteceu uma indisponiblidade no ambiente Kubernetes de lá, por conta de certificados expirados.
Ele esta demasiadamente preocupado.
Ele quer que tenhamos a certeza que nosso cluster não corre esse perigo, portanto, adicione no arquivo /tmp/meus-certificados.txt todos eles e suas datas de expiração.

### Respostas

Os certificados, por padrão, ficam no diretório /etc/kubernetes/pki, para que você possa verificar a data de experação, você pode utilizar o **openssl** conforme abaixo:

```bash
cd /etc/kubernetes/pki/
openssl x509 -noout -text -in apiserver.crt | grep -A3 Validity
```

> Lembrar de adicionar a data de expiração no arquivo solicitado na questão.

Caso queira fazer de uma forma mais bonitinha, faça conforme abaixo:

```bash
find /etc/kubernetes/pki/ -iname "apiserver*crt" -exec openssl x509 -noout -subject -enddate -in {} \; > /tmp/meus-certificados.txt 
```

Uma outra forma de conseguirmos tais informações, é chamando o seguinte comando:

```bash
kubeadm certs check-expiration
```

Podemos redirecionar a saida para o nosso arquivo /tmp/**meus-certificados.txt**

### Questão 3

Pois bem, vimos que precisamos atualizar o nosso cluster imediatamente, sem trazer nenhuma indisponibilidade para o ambiente. Como devemos proceder:

### Respostas

Podemos utilizar o comando kubeadm certs para vizualizar as datas corretas e também para realizar sua renovação. Conforme descrito abaixo:

```bash
kubeadm certs renew all
```

> Lembrando a importancia de realizar o procedimento em todos os nodes master.

> Lembre se de reiniciar o apiserver, controller, scheduller e o etcd.

> Para isso, você pode utilizar o comando docker stop, de dentro do node que esta sendo atualizado.
