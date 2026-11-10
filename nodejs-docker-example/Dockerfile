
# syntax=docker/dockerfile:1

FROM node:24-alpine AS base

WORKDIR /app

COPY package*.json ./

# Install all dependencies
FROM base AS dev

RUN npm ci

COPY . .

CMD ["npm", "run", "dev"]

# Build application
FROM base AS builder

RUN npm ci

COPY . .

RUN npm run build

# Production image
FROM node:24-alpine AS runner

WORKDIR /app

ENV NODE_ENV=production

COPY package*.json ./

RUN npm ci --omit=dev

COPY --from=builder /app/dist ./dist

EXPOSE 3000

CMD ["node", "dist/index.js"]

# Run tests
FROM dev AS test

ENV CI=true

CMD ["npm", "test"]