# Stage 1 - build
FROM node:20-alpine AS build
WORKDIR /app
COPY package*.json ./

# Use npm ci if package-lock.json exists, otherwise npm install
RUN if [ -f package-lock.json ]; then npm ci; else npm install; fi

COPY . .
ARG REACT_APP_DEV_BACKEND
ARG REACT_APP_PROD_BACKEND
ENV REACT_APP_DEV_BACKEND=$REACT_APP_DEV_BACKEND
ENV REACT_APP_PROD_BACKEND=$REACT_APP_PROD_BACKEND
RUN npm run build

# Stage 2 - serve with nginx
FROM nginx:stable-alpine
COPY --from=build /app/build /usr/share/nginx/html
# change default listen port to 3000 inside container
RUN sed -i 's/listen       80;/listen       3000;/' /etc/nginx/conf.d/default.conf

EXPOSE 3000
CMD ["nginx", "-g", "daemon off;"]