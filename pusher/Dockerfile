FROM node:14.15.3-buster-slim AS builder

COPY messages /build/messages
COPY pusher /build/pusher
RUN \
	set -ex ;\
	apt-get update ;\
	apt-get install -y --no-install-recommends python3 build-essential ;\
	cd /build/messages ;\
	yarn install

RUN set -ex ;\
	cd /build/messages ;\
	yarn run proto ;\
	mv generated ../pusher/src/Messages/generated ;\
	cd /build/pusher ;\
	yarn install ;\
	yarn run tsc

FROM node:14.15.3-buster-slim
WORKDIR /app
COPY --from=builder /build/pusher/ .
CMD ["yarn", "run", "runprod"]
