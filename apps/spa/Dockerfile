FROM denoland/deno:2.1.5

EXPOSE 5173

WORKDIR /app

COPY . .

RUN deno install


WORKDIR /app/apps/spa


CMD [ "deno", "task", "start"  ]
