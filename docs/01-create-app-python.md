# 🚀 Flask + Kubernetes (Kind) + GitHub Actions

## **Criar a Aplicação Flask**  
1. Criar um diretório para o projeto e entrar nele:  
   ```sh
   mkdir flask-k8s && cd flask-k8s
   ```  
2. Criar um ambiente virtual (opcional, mas recomendado):  
   ```sh
   python -m venv venv
   source venv/bin/activate  # No Windows: venv\Scripts\activate
   ```  
3. Instalar o Flask:  
   ```sh
   pip install flask
   ```  
4. Criar o arquivo `app.py` com o seguinte código:  
   ```python
   from flask import Flask

   app = Flask(__name__)

   @app.route("/")
   def hello():
       return "Hello, World!"

   if __name__ == "__main__":
       app.run(host="0.0.0.0", port=5000)
   ```  
