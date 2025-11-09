# Use uma imagem Python oficial
FROM python:3.11-slim

# Define o diretório de trabalho
WORKDIR /app

# Instala dependências do sistema (necessárias para psycopg2 e outras libs)
RUN apt-get update && apt-get install -y --no-install-recommends \
    gcc \
    libpq-dev \
    && rm -rf /var/lib/apt/lists/*

# Copia o arquivo de dependências
COPY requirements.txt .

# Instala as dependências Python
RUN pip install --no-cache-dir --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt

# Copia todo o código da aplicação
COPY . .

# Cria usuário não-root para segurança
RUN useradd -m -u 1000 appuser && chown -R appuser:appuser /app
USER appuser

# Expõe a porta (Fly.io usa a variável PORT, padrão é 8080)
EXPOSE 8080

# Comando para rodar a aplicação
# Fly.io define a variável PORT automaticamente
CMD sh -c "uvicorn api.main:app --host 0.0.0.0 --port ${PORT:-8080}"

