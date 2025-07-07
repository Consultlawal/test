# Stage 1: Build stage
FROM python:3.11-slim as builder

WORKDIR /app

# Copy requirements and install into user space
COPY requirements.txt .
RUN pip install --user --no-warn-script-location -r requirements.txt

# Stage 2: Final runtime image
FROM python:3.11-slim

WORKDIR /app

# Copy installed packages from builder
COPY --from=builder /root/.local /root/.local
ENV PATH=/root/.local/bin:$PATH

# Copy source code
COPY . .

EXPOSE 8010
CMD ["uvicorn", "server:app", "--host", "0.0.0.0", "--port", "8010"]