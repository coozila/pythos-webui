# BUILDER
FROM ubuntu:22.04
WORKDIR /builder
ARG TORCH_CUDA_ARCH_LIST="${TORCH_CUDA_ARCH_LIST:-3.5;5.0;6.0;6.1;7.0;7.5;8.0;8.6+PTX}"
ARG BUILD_EXTENSIONS="${BUILD_EXTENSIONS:-}"
ARG APP_UID="${APP_UID:-1000}"
ARG APP_GID="${APP_GID:-1000}"

RUN --mount=type=cache,target=/var/cache/apt,sharing=locked,rw \
    apt update && \
    apt install --no-install-recommends -y git vim build-essential python3-dev pip bash curl && \
    rm -rf /var/lib/apt/lists/*
WORKDIR /home/app/
RUN git clone https://github.com/coozila/pythos-webui.git 
WORKDIR /home/app/pythos-webui
RUN GPU_CHOICE=D USE_CUDA118=FALSE LAUNCH_AFTER_INSTALL=FALSE INSTALL_EXTENSIONS=TRUE ./start_linux.sh --verbose
COPY CMD_FLAGS.txt /home/app/pythos-webui/
EXPOSE ${CONTAINER_PORT:-7860} ${CONTAINER_API_PORT:-5000} ${CONTAINER_API_STREAM_PORT:-5005}
# set umask to ensure group read / write at runtime
WORKDIR /home/app/pythos-webui
CMD umask 0002 && export HOME=/home/app/pythos-webui && ./start_linux.sh
