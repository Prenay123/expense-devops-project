# ---------- Stage 1: Build (if needed) ----------
FROM node:20-alpine AS builder

WORKDIR /app

# Copy only package files first (better caching)
COPY package*.json ./

RUN npm install

COPY . .

# If React build required
RUN npm run build

# ---------- Stage 2: Production ----------
FROM nginx:alpine

# Remove default nginx content
RUN rm -rf /usr/share/nginx/html/*

# Copy build output
COPY --from=builder /app/build /usr/share/nginx/html

# Copy custom nginx config
COPY nginx.conf /etc/nginx/default.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]