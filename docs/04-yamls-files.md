## **4️⃣ Criar Manifests Kubernetes**  
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
