FROM node:18

WORKDIR /opt/app

# Stay as root for system installations
USER root

# Install sqlite3 using apt-get
RUN apt-get update && \
    apt-get install -y sqlite3 && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Create data directory and set permissions
RUN mkdir -p /opt/app/data && \
    chown -R node:node /opt/app && \
    chmod 777 /opt/app/data

COPY --chown=node:node ./node/package*.json /opt/app/

# Switch to node user after system setup
USER node
RUN npm install

COPY --chown=node:node ./node/index.js ./
COPY --chown=node:node ./node/db.js ./
COPY --chown=node:node ./.env ./

EXPOSE 8000

# Create a shell script to ensure proper permissions and start node
RUN echo '#!/bin/sh\nmkdir -p /opt/app/data\nchmod 777 /opt/app/data\nnode index.js' > /opt/app/start.sh && \
    chmod +x /opt/app/start.sh

CMD ["/opt/app/start.sh"]
