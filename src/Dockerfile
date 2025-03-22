# Estágio de construção (build stage)
FROM python:3.11.0 AS builder

# Copia o arquivo de requisitos
COPY requirements.txt .

# Instala as dependências em um diretório temporário
RUN python -m pip install --user -r requirements.txt

# Estágio final (production stage)
FROM python:3.11.0-slim

# Expõe a porta 5000
EXPOSE 5000

# Instala as dependências de sistema necessárias para o psycopg2
RUN apt-get update && apt-get install -y --no-install-recommends \
    libpq-dev \
    && rm -rf /var/lib/apt/lists/*

# Copia apenas as dependências instaladas do estágio de construção
COPY --from=builder /root/.local /root/.local

# Define o diretório de trabalho
WORKDIR /app

# Copia o conteúdo do diretório atual para o diretório de trabalho no contêiner
COPY . /app

# Garante que o entrypoint.sh seja executável
RUN chmod +x /app/entrypoint.sh

# Define a variável de ambiente para o Prometheus
ENV PROMETHEUS_MULTIPROC_DIR=/tmp/metrics

# Cria o diretório para métricas
RUN mkdir -p /tmp/metrics

# Adiciona o diretório de instalação das dependências ao PATH
ENV PATH=/root/.local/bin:$PATH

# Comando para rodar o entrypoint.sh
CMD ["/app/entrypoint.sh"]