# Stage 1: Build
FROM node:20-alpine AS build
WORKDIR /app
COPY package.json yarn.lock ./
RUN yarn install
COPY . .
RUN yarn build

# Stage 2: Get the static server binary
FROM ghcr.io/static-web-server/static-web-server:2-alpine AS server

# Stage 3: Final scratch image
FROM scratch
COPY --from=server /usr/local/bin/static-web-server /static-web-server
COPY --from=build /app/build /public
EXPOSE 8787
ENTRYPOINT ["/static-web-server", "--root", "/public", "--port", "8787", "--page-fallback", "/public/index.html"]
