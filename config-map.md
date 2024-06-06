Claro, aqui está como você pode usar um ConfigMap com o NGINX e Kubernetes, e depois verificar o conteúdo do arquivo dentro do container:

1. **Crie um ConfigMap**: Você pode usar o comando `kubectl create configmap` para criar um ConfigMap a partir de valores literais, diretórios ou arquivos. Por exemplo, se você tem um arquivo de configuração do NGINX chamado `nginx.conf`, você pode criar um ConfigMap com ele da seguinte maneira:

```bash
kubectl create configmap confnginx --from-file=./data/nginx.conf
```

2. **Use o ConfigMap em um Pod**: Você pode usar o ConfigMap em um Pod montando-o como um volume. Abaixo está um exemplo de um manifesto de implantação com o ConfigMap montado como um volume no único container do Pod:

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

3. **Verifique o conteúdo do arquivo dentro do container**: Para verificar o conteúdo do arquivo dentro do container, você pode usar o comando `kubectl exec`. Primeiro, obtenha o nome do Pod:

```bash
kubectl get pods -l app.kubernetes.io/name=configmap-volume
```

Em seguida, execute o comando `cat` dentro do container:

```bash
kubectl exec -it <nome-do-pod> -- cat /etc/nginx/nginx.conf
```

Substitua `<nome-do-pod>` pelo nome real do seu Pod. Isso exibirá o conteúdo do arquivo `nginx.conf` dentro do container.

Espero que isso ajude a entender como usar ConfigMaps com Kubernetes e NGINX. Se você tiver mais perguntas, fique à vontade para perguntar!
