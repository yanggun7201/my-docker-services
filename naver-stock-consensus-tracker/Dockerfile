FROM ghcr.io/puppeteer/puppeteer:24

WORKDIR /app

COPY package.json package-lock.json ./
RUN npm ci

COPY . .

CMD ["node", "index.js"]
