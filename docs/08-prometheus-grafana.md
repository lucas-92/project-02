## **8 Adicionar Monitoramento com Prometheus e Grafana**  
1. Instalar Prometheus e Grafana via Helm:  
   ```sh
   helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
   helm repo update
   helm install prometheus prometheus-community/kube-prometheus-stack
   ```  
2. Verificar os pods:  
   ```sh
   kubectl get pods -n default
   ```  
3. Configurar port-forward para Grafana:  
   ```sh
   kubectl port-forward svc/prometheus-grafana 3000:80 -n default
   ```  
4. Acessar **Grafana** em [http://localhost:3000](http://localhost:3000)  
   - Login: **admin** / Senha: **prom-operator**  
5. Adicionar o Prometheus como fonte de dados. 
