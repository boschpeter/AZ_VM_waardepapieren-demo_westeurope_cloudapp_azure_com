FROM node:10
RUN mkdir /app
ADD package.json package-lock.json /app/
ENV REACT_APP_EPHEMERAL_ENDPOINT=https://waardepapieren-demo.westeurope.cloudapp.azure.com:443/api/eph
ENV REACT_APP_EPHEMERAL_WEBSOCKET_ENDPOINT=wss://waardepapieren-demo.westeurope.cloudapp.azure.com:443/api/eph-ws
WORKDIR /app
RUN npm install --unsafe-perm
ADD public /app/public
ADD src /app/src
ARG CERTIFICATE_HOST
ENV REACT_APP_CERTIFICATE_HOST=http://waardepapieren-demo.westeurope.cloudapp.azure.com:8880
RUN npm run build

FROM nginx:1.15.8
ADD nginx/nginx.conf /etc/nginx/nginx.conf
COPY --from=0 /app/build /usr/share/nginx/html
#  volumes:
#    - ./clerk-frontend/nginx/certs:/etc/nginx/certs:rw
RUN mkdir /etc/nginx/certs
ADD nginx/certs/org.crt /etc/nginx/certs/org.crt
ADD nginx/certs/org.key /etc/nginx/certs/org.key
