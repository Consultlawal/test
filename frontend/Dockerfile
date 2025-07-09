# Stage 1: Builder
FROM node:18 AS builder

WORKDIR /app

# Install dependencies
COPY package*.json ./
RUN npm install

# Copy app source code
COPY . .

# Build Next.js app
RUN npm run build

# Stage 2: Runtime
FROM node:18-alpine AS stage-1

WORKDIR /app

# Copy only necessary files from builder
COPY --from=builder /app/.next ./.next/
COPY --from=builder /app/package*.json ./
COPY --from=builder /app/node_modules ./node_modules/

# Expose Next.js port
EXPOSE 3000

# Start the app
CMD ["npm", "run", "start"]
