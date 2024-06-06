ConfigMaps são uma maneira de configurar containers dentro de um Pod no Kubernetes. Eles podem ser usados de quatro maneiras diferentes⁶:

1. Dentro de um comando e argumentos de um container
2. Como variáveis de ambiente para um container
3. Adicionar um arquivo em um volume somente leitura, para a aplicação ler
4. Escrever código para rodar dentro do Pod que usa a API do Kubernetes para ler um ConfigMap

Aqui está um exemplo de como você pode usar um ConfigMap com o NGINX:

1. **Crie um ConfigMap**: Você pode usar o comando `kubectl create configmap` para criar um ConfigMap a partir de valores literais, diretórios ou arquivos. Por exemplo, se você tem um arquivo de configuração do NGINX chamado `nginx.conf`, você pode criar um ConfigMap com ele da seguinte maneira⁷:

```bash
kubectl create configmap confnginx --from-file=./data/nginx.conf
```

2. **Use o ConfigMap em um Pod**: Você pode usar o ConfigMap em um Pod montando-o como um volume. Abaixo está um exemplo de um manifesto de implantação com o ConfigMap montado como um volume no único container do Pod⁵:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: configmap-volume
  labels:
    app.kubernetes.io/name: configmap-volume
spec:
  replicas: 3
  selector:
    matchLabels:
      app.kubernetes.io/name: configmap-volume
  template:
    metadata:
      labels:
        app.kubernetes.io/name: configmap-volume
    spec:
      containers:
      -  name: nginx
         image: nginx
         ports:
         -  containerPort: 80
         volumeMounts:
         -  name: config-volume
            mountPath: /etc/nginx
      volumes:
      -  name: config-volume
         configMap:
           name: confnginx
```

Neste exemplo, o ConfigMap `confnginx` é montado como um volume no caminho `/etc/nginx` dentro do container NGINX. O NGINX pode então usar o arquivo de configuração fornecido pelo ConfigMap.

3. **Atualize a Configuração**: Se você precisar atualizar a configuração, você pode atualizar o ConfigMap. O Kubernetes irá atualizar automaticamente o volume montado com a nova configuração⁵.

Espero que isso ajude a entender como usar ConfigMaps com Kubernetes e NGINX. Se você tiver mais perguntas, fique à vontade para perguntar!

Fonte: conversa com o Copilot, 06/06/2024
(1) ConfigMaps | Kubernetes. https://kubernetes.io/docs/concepts/configuration/configmap/?force_isolation=true.
(2) Add nginx.conf to Kubernetes cluster - Stack Overflow. https://stackoverflow.com/questions/42078080/add-nginx-conf-to-kubernetes-cluster.
(3) Updating Configuration via a ConfigMap | Kubernetes. https://kubernetes.io/docs/tutorials/configuration/updating-configuration-via-a-configmap/.
(4) CRIANDO UM CLUSTER K8S COM NGINX INGRESS CONTROLLER EM SUA MÁQUINA!. https://www.youtube.com/watch?v=1lx91nhzNe0.
(5) #06 Configuração do K8s - Curso de Introdução ao Kubernetes. https://www.youtube.com/watch?v=V1g0alYqI1w.
(6) Descubra como configurar o NGinx: AULA COMPLETA com dicas de segurança e performance. https://www.youtube.com/watch?v=WeoZ4Ego1vs.
(7) Configurando um Pod Para Usar um ConfigMap | Kubernetes. https://kubernetes.io/pt-br/docs/tasks/configure-pod-container/configure-pod-configmap/.
(8) Kubernetes: Deploying a customized Nginx web server with configmap. https://towardsaws.com/kubernetes-deploying-a-customized-nginx-web-server-with-configmap-e47ec0a2f0c7.
(9) undefined. https://www.linuxtips.io/.
(10) undefined. https://www.getup.io/kubicast.
(11) undefined. https://k8s.io/examples/deployments/deployment-with-configmap-as-volume.yaml.
