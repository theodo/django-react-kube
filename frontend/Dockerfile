# Step 1: collect django statics
FROM node:12-alpine as builder
WORKDIR /usr/app
COPY . .
RUN yarn install
ENV REACT_APP_ENV=production
# Change me!
ENV REACT_APP_API_BASE_URL=kube-django.com/api
ENV NODE_PATH=src/
RUN yarn build

# Step 2: Put statics where they can be served by Nginx
FROM nginx:1.17-alpine
COPY --from=builder /usr/app/build /usr/share/nginx/html
