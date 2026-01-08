# syntax=docker/dockerfile:1.7

FROM mcr.microsoft.com/devcontainers/python:3.12-bookworm

# --- OS packages (keep minimal for build speed) ---
RUN --mount=type=cache,target=/var/cache/apt,sharing=locked \
    --mount=type=cache,target=/var/lib/apt,sharing=locked \
    apt-get update && \
    apt-get install -y --no-install-recommends \
      build-essential \
      curl \
      git \
      ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# --- Install uv (official container image) ---
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

