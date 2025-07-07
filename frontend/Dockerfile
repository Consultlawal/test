# Stage 1: Build stage
FROM node:18 as builder

WORKDIR /app

# Copy package.json and install dependencies
COPY package*.json ./
RUN npm install

# Copy source files and build app (if needed)
COPY . .
RUN npm run build  # Optional if you have a build step

# Stage 2: Final minimal runtime image
FROM node:18-alpine

WORKDIR /app

# Copy only necessary files from builder
COPY --from=builder /app/package*.json ./
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/dist ./dist/

EXPOSE 3000
CMD ["npm", "run", "dev"]