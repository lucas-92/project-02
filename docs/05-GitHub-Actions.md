## **5️⃣ Configurar CI/CD com GitHub Actions**  
Criar um diretório `.github/workflows/` e adicionar o arquivo `ci-cd.yaml`:  

```yaml
name: CI/CD Pipeline

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout do código
      uses: actions/checkout@v3

    - name: Build da imagem Docker
      run: |
        docker build -t flask-app:latest .

    - name: Teste da aplicação
      run: |
        docker run -d -p 5000:5000 flask-app
        sleep 5
        curl -f http://localhost:5000 || exit 1
```  
