# Kubernetes  


* Kubernetes é capaz de criar e gerenciar um container. Por isso é um gerenciador de containers.   
* Define qual a melhor máquina é responsável por executar um container.  
* Caso um container falhe, o Kubernetes pode substituir por outro container.  
* Caso a esgote a capacidade computacional de uma máquina, ele é capaz  de substituir está por outra.  

## Master e Nodes    
Denominações das máquinas dentro do cluster      

#### Master   
* Gernciar Cluster   
* Manter e atualizar o estado desejado    
* Receber e executar novos comandos   

#### Node  
* Executar as aplicações  


### Control Plane e Nodes

### Comunicação  

Através do comando `kubectl` posso criar, ler, remover e atualizar se comunicando com cluster através da api do K ubernetes que recebe e envia requisições para os recursos como: __pod__, __rs__, __deploy__ e __volumes__.   


### Instalação 

```bash
kubectl:

kubectl version --client

```


### minikube:   
#### Criar cluster Kubernetes com o Minikube:  

```bash
minikube start --vm-driver=virtualbox  

```
### Verificar node criado:   
  
```bash
kubectl get nodes
```   


### Pods

* No universo __Docker__ gerenciamos Containers, quando que no __Kubernetes__ trabalhamos com Pods.  
* Um Pod é uma capsula que pode conter 1 ou mais containers. 
* Sempre que criamos um pod ganhamos um endereço IP.
* Dentro de um pod, podemos definir a porta que necessitamos para cada container.
* Caso um container falhe dentro de um Pod, outro será criado.Os pods são efemeros.
* Dentro de um Pod os containers compartilham namespaces de rede e IPC. Também podem compartilhar volumes.
* Como possuem IP's diferentes, containers em pods diferentes podem utilizar o mesmo número de porta.  
* Containers dentro de um mesmo pod conseguem se comunicar via localhost.

### Como criar um Pod?  
#### Exemplo. Criação de um Pod com a imagem do Nginx:  


```bash
kubectl run nginx-server --image=nginx:latest
```  

### Verficar Pods criado:   
```bash
kubectl get pods
```  

### para acompanhar todas as ocorrências em execução:  

```bash
kubectl get pods --watch  
```   

### Verificar os detalhes do Pod gerado:  


```bash
kubectl describe pods <nome_do_pods>
```  

### Atualizar a versão da imagem no Pod:   
#### A saída do comando abrirá o Vim para editar de maneira declarativa   
  
```bash
kubectl edit pod <nome-do-pod>

```
### Pods declarivo
###  Cria um arquivo yml como o presente neste projeto.
### Informar as chaves e valores de acordo com a configuração desejada. Ao terminar, rodar o seguinte comando:  

```bash
kubectl apply -f primeiro-pod.yml 
```

### Para verificar:
```bash
kubectl describe pods primeiro-pod-declarivo
```
### vAcessar container dentro do Pod  
```bash
kubectl exec -it portal-noticias -- bash
```   

### Services (SVC)  

* Abstrações para expor aplicações em um ou mais pods  
* Proveem IP's fixos para comunicação.(Isto é importante, pois caso um container morra outro é criado automaticamente, mas com endereço de ip distinto e para não impactar na aplicação, p SVC cria um DNS fixo.) 
* Proveem DNS para um ou mais pods.  
* São capazes de fazer balanceamento de carga.  

* __CLuster IP__    

* Responsável por fazer a comunicação entre diferente pods dentro de um cluster  
* Após criar o arquivo de configuração do Service, como o svc-pod-2.yaml e confirgurar a label no arquivo do kind, posso listar os servives do cluste com:

`kubectl get svc`   

* Através de `labels` definidas no `metadata` e utilizando o campo `selector` no service, configurados no arquivo yaml. O service sabe qual pods deve gerenciar.  

* Estudar os arquivos `svc-pod-2.yml` e o `pod-2.yml`. Os valores do segundo são informados ao primeiro arquivo.

* __NodePort__    

* NodePort, abre comunicação com o mundo externo. Ou seja, a partir dele com o navegador, podemos acessar o endereço em que está a aplicação que está dentro do cluster.    

* Devemos criar um service NodePort, como foi criado no arquivo `svc-pod-1.yml` passando o valor `NodePort` para a chave `type`.  

obs: pressupõe que foi configurado o `label` e `app` no arquivo `pod-1.yaml`.  

Após a configuração: executar os comandos e teremos as respectivas saídas:   

`kubectl get svc`  
![image](./image/get-svc.png)   

`kubectl get node -o wide`  
![image](./image/node-o_wide.png)

Na primeira imagem iremos, buscamos a porta configurada no __NodePort__(o IP do Node). Na segunda, o endereço de IP do __MiniKube__. Teremos: `192.168.99.114:30000`  

O resultado esperado é o acesso à página do Nginx.

* __LoadBalancer__    