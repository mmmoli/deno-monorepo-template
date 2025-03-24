FROM debian:bullseye-slim AS tools

RUN apt-get update && apt-get install -y curl
RUN curl -sfS "https://dotenvx.sh?directory=/home" | sh

FROM denoland/deno:2.2.5 AS build-env

COPY --from=tools /home/dotenvx /home/dotenvx
COPY . /repo/
WORKDIR /repo
RUN deno install
RUN deno task compile:web

FROM gcr.io/distroless/cc:nonroot
COPY --from=tools /home/dotenvx /home/nonroot/dotenvx
COPY --from=build-env /repo/apps/web/build/client/ /home/nonroot/build/client/
COPY --from=build-env /repo/apps/web/dist/server /home/nonroot/server
COPY --from=build-env /repo/.env.production /home/nonroot/.env.production

WORKDIR /home/nonroot

CMD ["./dotenvx", "run", "-f .env.production", "--", "./server"]
