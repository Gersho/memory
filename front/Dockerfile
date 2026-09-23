# Build Stage
FROM node:20-alpine AS build
WORKDIR /app
ARG REACT_APP_API_URL
ENV REACT_APP_API_URL=${REACT_APP_API_URL}

COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Production Web Server
FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
# Custom Nginx config to support React client-side routing
RUN echo 'server { listen 80; location / { root /usr/share/nginx/html; index index.html; try_files $uri /index.html; } }' > /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]