ARG BUILD_IMAGE=node:latest
FROM ${BUILD_IMAGE}

# Change timezone
RUN echo "Asia/Shanghai" > /etc/timezone && \
    dpkg-reconfigure -f noninteractive tzdata && \
    npm config set registry https://registry.npm.taobao.org

# Create app directory
WORKDIR /data/project

# Install app dependencies
COPY package.json ./
RUN npm install --only=production && npm cache clean --force

# Bundle app source
COPY ./ ./

CMD ["npm","start"]
