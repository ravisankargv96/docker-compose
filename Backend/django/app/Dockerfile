# syntax=docker/dockerfile:1.4

FROM --platform=$BUILDPLATFORM python:3.7-alpine AS builder
EXPOSE 8000
WORKDIR /app 
COPY requirements.txt /app
RUN pip3 install -r requirements.txt --no-cache-dir
COPY . /app 
ENTRYPOINT ["python3"] 
CMD ["manage.py", "runserver", "0.0.0.0:8000"]

FROM builder as dev-envs
RUN <<EOF
apk update
apk add git
EOF

RUN <<EOF
addgroup -S docker
adduser -S --shell /bin/bash --ingroup docker vscode
EOF
# install Docker tools (cli, buildx, compose)
COPY --from=gloursdocker/docker / /
CMD ["manage.py", "runserver", "0.0.0.0:8000"]


# 1. Upto the First part it's a builder stage
#    run: python3 manage.py runserver 0.0.0.0:8000

# 2. In second part it's a dev-envs stage
#    a. update the pkgs & install git.
#    b. Installs vscode+Docker with terminal /bin/bash & grouped name = docker.
#    c. Docker related tools (cli, buildx, compose) present in gloursdocker/docker are copied
#    d. python3 manage.py runserver 0.0.0.0:8000