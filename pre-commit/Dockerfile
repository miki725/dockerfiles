FROM python:3.7-slim

RUN apt-get update \
    && \
    apt-get install -y \
        git \
        gcc \
    && \
    apt-get clean \
    && \
    rm -rf /var/lib/apt/lists/* \
    && \
    python3 -m pip install --no-cache-dir \
        pre-commit

CMD pre-commit run --all-files
