#########################################################################################
## docker will invoke this file from ../.. dir in order to access code
#########################################################################################
FROM caddy:2.11-alpine AS runner

COPY python-api-examples/web/*.html /usr/share/caddy/
COPY python-api-examples/web/css /usr/share/caddy/css
COPY python-api-examples/web/js /usr/share/caddy/js
COPY python-api-examples/web/pic /usr/share/caddy/pic
