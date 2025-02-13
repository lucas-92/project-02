## **3 Publicar a Imagem no Docker Hub ou GitHub Container Registry**  
1. Criar uma conta no **Docker Hub** ou ativar o **GitHub Container Registry (GHCR)**.  
2. Fazer login no registro de contêiner:  
   ```sh
   docker login -u seu-usuario
   ```  
3. Criar uma tag e enviar a imagem para o repositório:  
   **Docker Hub:**  
   ```sh
   docker tag flask-app seu-usuario/flask-app:latest
   docker push seu-usuario/flask-app:latest
   ```  
   **GitHub Container Registry:**  
   ```sh
   docker tag flask-app ghcr.io/seu-usuario/flask-app:latest
   docker push ghcr.io/seu-usuario/flask-app:latest
   ```  
4. Atualizar o `deployment.yaml` para usar a imagem publicada:  
   ```yaml
   containers:
   - name: flask-app
     image: seu-usuario/flask-app:latest
   ```  
5. Reaplicar os manifests:  
   ```sh
   kubectl apply -f k8s/
