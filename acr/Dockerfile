FROM nginx:stable-alpine 
RUN rm /usr/share/nginx/html/index.html && echo "hello nginx" > /usr/share/nginx/html/index.html
CMD ["nginx", "-g", "daemon off;"]
