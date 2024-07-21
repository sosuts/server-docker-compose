FROM debian:bullseye-slim AS builder

WORKDIR /opt

ENV RYE_HOME="/opt/rye"
ENV PATH="$RYE_HOME/shims:$PATH"

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    ca-certificates \
    curl

SHELL [ "/bin/bash", "-o", "pipefail", "-c" ]
RUN curl -sSf https://rye.astral.sh/get | RYE_INSTALL_OPTION="--yes" bash && \
    rye config --set-bool behavior.global-python=true && \
    rye config --set-bool behavior.use-uv=true


FROM mcr.microsoft.com/vscode/devcontainers/base:bullseye
COPY --from=builder /opt/rye /opt/rye
ENV RYE_HOME="/opt/rye"
ENV PATH="$RYE_HOME/shims:$PATH"
ENV PYTHONUNBUFFERED True
RUN rye config --set-bool behavior.global-python=true && \
    rye config --set-bool behavior.use-uv=true

RUN rye init && rye add streamlit && rye sync

RUN chown -R vscode $RYE_HOME

# activate rye venv and run streamlit
CMD [ "rye", "run", "streamlit", "hello" ]
