# Dockerfile for React app

# Step 1: Build the React app
FROM node:18-bookworm-slim AS build
WORKDIR /app
COPY package.json ./
COPY package-lock.json ./
RUN npm install
COPY . ./
RUN npm run build

# Step 2: Serve the built app using npm
FROM nginx:alpine

COPY --from=build /app/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]

# CMD npm run dev -- -- host 0.0.0.0
