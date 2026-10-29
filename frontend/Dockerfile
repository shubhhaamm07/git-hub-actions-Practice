# Stage 1 — build the app
FROM dhi.io/node:26-dev AS build
WORKDIR /app

COPY package*.json ./
RUN npm ci --legacy-peer-deps

COPY . .
RUN npm run build

# Stage 2 — static file server
FROM dhi.io/nginx:1 
COPY --from=build /app/dist /usr/share/nginx/html
EXPOSE 8080

FROM dhi.io/node:26
WORKDIR /app
COPY --from=build /app/dist ./dist
COPY --from=build /app/node_modules ./node_modules
COPY vite.preview.config.js ./vite.config.js
EXPOSE 4173
ENTRYPOINT ["node", "--no-warnings"]
CMD ["./node_modules/.bin/vite", "preview", "--host", "0.0.0.0", "--port", "4173"]