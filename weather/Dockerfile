# Use Python 3.12 slim image as base
FROM python:3.12-slim

# Set working directory
WORKDIR /app

# Install curl for downloading uv
RUN apt-get update && apt-get install -y curl && rm -rf /var/lib/apt/lists/*

# Install uv for faster dependency management
RUN curl -LsSf https://astral.sh/uv/install.sh | sh
ENV PATH="/root/.local/bin:$PATH"

# Copy project files
COPY pyproject.toml uv.lock* ./
COPY weather.py ./

# Install dependencies using uv
RUN uv sync --frozen --no-cache

# Set environment variables
ENV PYTHONUNBUFFERED=1
ENV PATH="/app/.venv/bin:$PATH"

# Expose stdio for MCP communication
# MCP servers communicate via stdin/stdout, no ports needed

# Run the weather MCP server
CMD ["uv", "run", "weather"]

