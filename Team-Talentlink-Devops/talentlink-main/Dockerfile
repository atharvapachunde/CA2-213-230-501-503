# ---- Build stage ----
FROM node:20-alpine AS builder
WORKDIR /app

# Install deps
COPY package*.json ./
RUN npm ci

# Build app
COPY . .
RUN npm run build

# ---- Runtime stage ----
FROM node:20-alpine
ENV NODE_ENV=production
WORKDIR /app

# Copy runtime artifacts from builder
COPY --from=builder /app/package.json ./package.json
COPY --from=builder /app/.next ./\.next
COPY --from=builder /app/public ./public
COPY --from=builder /app/node_modules ./node_modules
# If your config is JS instead of MJS, change the next line accordingly
COPY --from=builder /app/next.config.mjs ./next.config.mjs

EXPOSE 3000
CMD ["npx", "next", "start", "-p", "3000"]
