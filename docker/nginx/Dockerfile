FROM nginx:alpine

# Copiar arquivos de configuações do Nginx
#COPY ./docker/nginx/conf/nginx.conf /etc/nginx/nginx.conf
COPY ./docker/nginx/conf/default.conf /etc/nginx/conf.d/default.conf
COPY ./frontend/vuejs/dist /usr/share/nginx/html

CMD ["nginx", "-g", "daemon off;"]