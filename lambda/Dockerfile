# Stage 1: Build stage
FROM python:3.9-slim as builder

WORKDIR /app

# Install dependencies into local dir
COPY requirements.txt .
RUN pip install --user --no-warn-script-location -r requirements.txt

# Stage 2: Final AWS Lambda image
FROM public.ecr.aws/lambda/python:3.9

# Copy installed packages
COPY --from=builder /root/.local/lib/python3.9/site-packages /var/task
COPY --from=builder /root/.local/bin /var/rapid/bin

# Copy function code
COPY lambda_function.py ./

CMD ["lambda_function.lambda_handler"]