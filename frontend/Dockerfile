FROM node:10.16.0-buster as build
# AWS cli tools
RUN apt-get update && apt-get install -y \
    build-essential \
    python \
    python-dev \
    python-pip
RUN pip install awscli

# Copy and install frontend requirements
COPY . /opt/frontend
WORKDIR /opt/frontend

ARG ENVIRONMENT
ARG LOCAL_ASSETS
ARG API_BASE_URL
ENV API_BASE_URL $API_BASE_URL
ENV ENVIRONMENT $ENVIRONMENT
ENV LOCAL_ASSETS $LOCAL_ASSETS
RUN yarn && \
    npm run build:dist

FROM nginx:1.17.8-alpine as nginx
COPY --from=build /opt/frontend/dist /usr/share/nginx/html
RUN rm /etc/nginx/conf.d/default.conf
COPY nginx.conf /etc/nginx/conf.d
CMD ["nginx", "-g", "daemon off;"]