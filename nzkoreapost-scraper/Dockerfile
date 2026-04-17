FROM node:22-alpine

RUN apk add --no-cache python3 make g++

WORKDIR /app

COPY package.json package-lock.json ./
RUN npm ci

COPY . .

RUN mkdir -p /app/data

CMD ["sh", "-c", "ln -sf /app/data/mydata.db /app/mydata.db && node index.js"]
