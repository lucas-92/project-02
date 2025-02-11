## **4 - Criar Manifests Kubernetes**  
Criar um diretório `k8s/` e adicionar os seguintes arquivos:  

### **Deployment (`deployment.yaml`)**  
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: flask-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: flask-app
  template:
    metadata:
      labels:
        app: flask-app
    spec:
      containers:
      - name: flask-app
        image: flask-app:latest
        ports:
        - containerPort: 5000
```  

### **Service (`service.yaml`)**  
```yaml
apiVersion: v1
kind: Service
metadata:
  name: flask-service
spec:
  selector:
    app: flask-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 5000
  type: NodePort
```  

1. Aplicar os manifests no cluster:  
   ```sh
   kubectl apply -f k8s/
   ```  
2. Verificar os pods:  
   ```sh
   kubectl get pods
   ```  
3. Testar a aplicação:  
   ```sh
   kubectl port-forward service/flask-service 8080:80
   curl http://localhost:8080

## Possíveis Erros

### Erro ao Fazer Port-Forward no Kubernetes
```
Forwarding from 127.0.0.1:8080 -> 5000
Forwarding from [::1]:8080 -> 5000
Handling connection for 8080
E0211 21:07:12.040638   55580 portforward.go:413] "Unhandled Error" err="an error occurred forwarding 8080 -> 5000: error forwarding port 5000 to pod 15b53a61f001a4c227588e97080eaad08a2ee443a1057f764bd46bd878e5b40c, uid : failed to execute portforward in network namespace \"/var/run/netns/cni-fe52227f-4af9-95fe-f088-17085b85cc09\": failed to connect to localhost:5000 inside namespace \"15b53a61f001a4c227588e97080eaad08a2ee443a1057f764bd46bd878e5b40c\", IPv4: dial tcp4 127.0.0.1:5000: connect: connection refused IPv6 dial tcp6 [::1]:5000: connect: connection refused "
error: lost connection to pod
```

Este erro indica que a tentativa de fazer **port-forwarding** de `8080` para `5000` falhou porque a conexão com a porta `5000` dentro do **pod** foi recusada.

### **Possíveis causas e soluções**

#### 1. A aplicação dentro do pod não está rodando ou escutando na porta correta
   - Entre no pod e verifique os processos ativos:  
     ```sh
     kubectl exec -it <nome-do-pod> -- netstat -tulnp
     ```
   - Se a porta `5000` não estiver em uso, inicie manualmente a aplicação dentro do pod ou corrija a configuração da porta.

#### 2. A aplicação está rodando em `localhost` ao invés de `0.0.0.0`
   - Se sua aplicação está rodando no Flask, por exemplo, inicie assim:  
     ```sh
     flask run --host=0.0.0.0 --port=5000
     ```
   - Isso garante que ela esteja acessível a partir de outras interfaces de rede dentro do pod.

#### 3. O container está crashando ou reiniciando
   - Verifique os logs do pod para entender o que está acontecendo:  
     ```sh
     kubectl logs <nome-do-pod>
     ```
   - Se o pod estiver sendo reiniciado constantemente, pode ser um erro na aplicação ou na configuração do container.

#### 4. O serviço Kubernetes não está configurado corretamente
   - Se sua aplicação roda dentro de um container, verifique se o serviço foi criado corretamente:  
     ```sh
     kubectl get svc
     ```
   - Se o serviço não estiver expondo a porta correta, edite a configuração.

#### 5. O pod não tem permissões suficientes para rodar o `port-forward`
   - Tente rodar o comando com `sudo` ou verifique permissões com:  
     ```sh
     kubectl auth can-i create pod/portforward
     ```

Se ainda estiver com problemas, verifique como sua aplicação está sendo executada (Dockerfile, Deployment, etc.) e ajuste as configurações necessárias. 🚀
