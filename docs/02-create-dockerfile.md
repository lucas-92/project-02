## **2️⃣ Criar o Dockerfile**  
1. Criar um arquivo `Dockerfile` na raiz do projeto:  
   ```dockerfile
   FROM python:3.9
   WORKDIR /app
   COPY requirements.txt requirements.txt
   COPY app.py app.py
   RUN pip install flask
   EXPOSE 5000
   CMD ["python", "app.py"]
   ```  
2. Criar um `requirements.txt` com:  
   ```
   flask
   ```  
3. Construir e testar o contêiner:  
   ```sh
   docker build -t flask-app .
   docker run -p 5000:5000 flask-app
   ```  
