**Habilitando Ingress Nginx no Minikube Localmente**

Aqui estão as etapas para habilitar o Ingress Nginx no Minikube localmente:

1. **Instale o Minikube**: Primeiro, você precisa ter o Minikube instalado em sua máquina. Se ainda não o fez, você pode baixá-lo do [site oficial do Minikube](https://minikube.sigs.k8s.io/docs/start/).

2. **Inicie o Minikube**: Use o seguinte comando para iniciar o Minikube:

    ```bash
    minikube start
    ```

3. **Habilite o Ingress**: O Minikube vem com o Ingress Nginx desabilitado por padrão. Para habilitá-lo, execute o seguinte comando:

    ```bash
    minikube addons enable ingress
    ```

4. **Confirme a Instalação**: Para confirmar que o Ingress Nginx está rodando, você pode executar o seguinte comando:

    ```bash
    kubectl get pods -n kube-system
    ```

    Você deve ver o controlador de ingresso nginx listado na saída.

5. **Configure o Ingress**: Agora você pode configurar o Ingress para o seu aplicativo. Você precisará criar um recurso Ingress em um arquivo yaml e aplicá-lo usando `kubectl apply -f`.

**Exemplo de um arquivo Ingress YAML**

Aqui está um exemplo de um arquivo Ingress YAML para um serviço chamado `web-service` no namespace `default`:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: my-app.my-domain.com
    http:
      paths:
      - pathType: Prefix
        path: "/"
        backend:
          service:
            name: web-service
            port:
              number: 80
```

**Configurando o DNS em uma Máquina Linux**

Para configurar o DNS em uma máquina Linux, você pode editar o arquivo `/etc/hosts`. Aqui estão os passos:

1. Abra o arquivo `/etc/hosts` com um editor de texto. Por exemplo, você pode usar o `nano`:

    ```bash
    sudo nano /etc/hosts
    ```

2. Adicione a seguinte linha ao arquivo, substituindo `192.168.99.100` pelo endereço IP do seu Minikube e `my-app.my-domain.com` pelo host do seu Ingress:

    ```
    192.168.99.100 my-app.my-domain.com
    ```

3. Salve e feche o arquivo. Agora, o seu sistema irá resolver `my-app.my-domain.com` para o endereço IP do seu Minikube.

Por favor, note que esses são apenas exemplos e você pode precisar ajustar os comandos de acordo com suas necessidades específicas. 
