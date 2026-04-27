# Use Nginx image
FROM nginx:alpine

# Copy your custom nginx config (The one we moved in Step 1)
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copy the built Angular files from dist folder
COPY dist/angular-16-crud /usr/share/nginx/html

EXPOSE 80
