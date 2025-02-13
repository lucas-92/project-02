## **4 Configurar o Kind**  
1. Criar um cluster Kubernetes com Kind:  
   ```sh
   kind create cluster --name flask-cluster
   ```  
2. Verificar se o cluster foi criado:  
   ```sh
   kubectl get nodes
