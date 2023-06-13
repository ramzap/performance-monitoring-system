FROM telegraf:1.24.4

WORKDIR /app
COPY entrypoint.sh ./
RUN chmod u+x entrypoint.sh
COPY telegraf.conf /etc/telegraf/telegraf.conf

ENTRYPOINT ["/app/entrypoint.sh"]
