# Usar a imagem base do Python
FROM python:3.9-slim

# Definir o diretório de trabalho no contêiner
WORKDIR /app

# Copiar o arquivo requirements.txt para o contêiner
COPY requirements.txt .

# Instalar as dependências do Python
RUN pip install --no-cache-dir -r requirements.txt

# Copiar o restante do código do aplicativo para o contêiner
COPY . .

# Expor a porta que o Flask usa
EXPOSE 59997

# Comando para executar o aplicativo
CMD ["python","app.py"]
