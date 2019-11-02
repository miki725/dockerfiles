FROM alpine

RUN apk add --no-cache \
        bash \
        gcc \
        git \
        nodejs-current \
        npm \
        python3 \
    && \
    python3 -m pip install --no-cache-dir \
        pre-commit

CMD pre-commit run --all-files
