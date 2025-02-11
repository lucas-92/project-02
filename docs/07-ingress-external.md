## **7 Criar um Ingress para Acesso Externo**  
1. Instalar um controlador de Ingress (Exemplo: NGINX):  
   ```sh
   kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
   ```  
2. Criar um arquivo `ingress.yaml`:  
   ```yaml
   apiVersion: networking.k8s.io/v1
   kind: Ingress
   metadata:
     name: flask-ingress
   spec:
     rules:
     - host: flask.local
       http:
         paths:
         - path: /
           pathType: Prefix
           backend:
             service:
               name: flask-service
               port:
                 number: 80
   ```  
3. Aplicar o Ingress:  
   ```sh
   kubectl apply -f k8s/ingress.yaml
   ```  
4. Adicionar `flask.local` ao `/etc/hosts`:  
   ```sh
   echo "127.0.0.1 flask.local" | sudo tee -a /etc/hosts
   ```  
5. Testar:  
   ```sh
   curl http://flask.local
   ```  
