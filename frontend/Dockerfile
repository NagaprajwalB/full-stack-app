# Next.js Frontend Dockerfile
FROM node:21-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build

FROM node:21-alpine

WORKDIR /app

COPY --from=builder /app/package*.json ./
COPY --from=builder /app/.next ./.next

RUN npm install --only=production

EXPOSE 3000

CMD ["npm", "start"]
