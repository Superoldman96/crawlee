FROM node:22 AS builder

COPY /packages ./packages
COPY /package*.json ./
RUN npm --quiet set progress=false \
    && npm install --only=prod --no-optional --no-audit \
    && npm update

FROM apify/actor-node-playwright-chrome:22-beta

RUN rm -r node_modules
COPY --from=builder /node_modules ./node_modules
COPY --from=builder /packages ./packages
COPY --from=builder /package*.json ./
COPY /.actor ./.actor
COPY /main.js ./

RUN echo "Installed NPM packages:" \
    && (npm list --only=prod --no-optional --all || true) \
    && echo "Node.js version:" \
    && node --version \
    && echo "NPM version:" \
    && npm --version
