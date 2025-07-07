# Stage 1: Build stage
FROM python:3.11-slim as builder

WORKDIR /app

# Copy requirements.txt first to leverage layer caching
COPY requirements.txt .

# Install dependencies into user space (~/.local)
RUN pip install --user --no-warn-script-location -r requirements.txt

# Stage 2: Final minimal runtime image
FROM python:3.11-slim

WORKDIR /app

# Install only necessary runtime system dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends netcat-openbsd libpq5 && \
    rm -rf /var/lib/apt/lists/*

# Copy installed packages from build stage
COPY --from=builder /root/.local /root/.local

# Add ~/.local/bin to PATH so installed Python binaries are available
ENV PATH=/root/.local/bin:$PATH

# Copy source code from local machine
COPY . /app

# Expose port and define CMD
EXPOSE 8000
CMD ["gunicorn", "myproject.wsgi:application", "--bind", "0.0.0.0:8000"]