FROM node:15.4.0-buster-slim AS build

COPY messages /build/messages
COPY front /build/front
RUN \
	set -ex ;\
	apt-get update ;\
	apt-get install -y --no-install-recommends python3 build-essential ;\
	cd /build/messages ;\
	yarn install

ARG ADMIN_URL
ARG API_URL
ARG UPLOADER_URL
ARG DEBUG_MODE=false
ARG JITSI_PRIVATE_MODE=false
ARG JITSI_URL
ARG TURN_SERVER=turn:coturn.workadventu.re:443,turns:coturn.workadventu.re:443
ARG TURN_USER=workadventure
ARG TURN_PASSWORD=WorkAdventure123

RUN set -ex ;\
	cd /build/messages ;\
	yarn run proto ;\
	mv generated ../front/src/Messages/generated ;\
	cd /build/front ;\
	yarn install ;\
	yarn run build

FROM httpd:2.4.46-alpine
COPY --from=build /build/front/dist/ /usr/local/apache2/htdocs/
