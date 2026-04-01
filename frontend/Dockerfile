# Build sur Debian : évite les échecs fréquents des binaires natifs (oxc, sharp, etc.) avec Alpine.
FROM node:24-bookworm-slim AS build

WORKDIR /app

ENV NPM_CONFIG_LEGACY_PEER_DEPS=true \
    NODE_OPTIONS="--max-old-space-size=6144" \
    NUXT_TELEMETRY_DISABLED=1

COPY package*.json ./

RUN npm ci --legacy-peer-deps

COPY . .

RUN npm run build

FROM node:24-alpine AS prod

WORKDIR /app

COPY --from=build /app/.output /app/.output

ENV NODE_ENV=production

EXPOSE 3000

CMD [ "node", ".output/server/index.mjs" ]
