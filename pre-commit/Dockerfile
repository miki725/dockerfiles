FROM alpine

RUN apk add --no-cache \
        python3 \
        nodejs-current \
        npm \
        bash \
        git && \
    python3 -m pip install --no-cache-dir \
        pre-commit

CMD pre-commit run --all-files
