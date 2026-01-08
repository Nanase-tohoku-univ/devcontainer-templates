FROM mcr.microsoft.com/devcontainers/python:3.12-bookworm

RUN --mount=type=cache,target=/var/cache/apt,sharing=locked \
    --mount=type=cache,target=/var/lib/apt,sharing=locked \
    apt-get update && \
    apt-get install -y --no-install-recommends \
      curl \
      git \
      vim \
      less \
      gcc \
      g++ \
      make \
      pkg-config \
      libgl1 \
      libglib2.0-0 \
      pulseaudio \
      alsa-utils \
      libasound2 \
      pulseaudio-utils \
    && rm -rf /var/lib/apt/lists/*

# --- Install uv (binary) ---
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /usr/local/bin/

# --- Project venv location (recommended in containers) ---
ENV UV_PROJECT_ENVIRONMENT=/opt/venv \
    VIRTUAL_ENV=/opt/venv \
    PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

# Build-time cache for uv downloads/wheels (root)
ENV UV_CACHE_DIR=/root/.cache/uv

# Pre-install dependencies using lockfile (fast rebuilds)
WORKDIR /tmp/project
COPY pyproject.toml uv.lock ./
RUN --mount=type=cache,target=/root/.cache/uv \
    uv sync --frozen --no-install-project

# Runtime uv cache dir を作成し、所有権を vscode に合わせる
RUN mkdir -p /home/vscode/.cache/uv \
    && chown -R vscode:vscode /home/vscode/.cache \
    && chown -R vscode:vscode /opt/venv

# Runtime env (vscode user + persistent cache volume)
USER vscode
ENV UV_CACHE_DIR=/home/vscode/.cache/uv \
    PATH="/opt/venv/bin:${PATH}"
