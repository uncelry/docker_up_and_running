FROM docker.io/bitnami/mongodb:4.4

COPY docker-healthcheck /usr/local/bin/

EXPOSE 27017
ENTRYPOINT ["/opt/bitnami/scripts/mongodb/entrypoint.sh"]
CMD ["/opt/bitnami/scripts/mongodb/run.sh"]

# Указываем как делать healthcheck для контейнеров из этого образа
HEALTHCHECK CMD ["docker-healthcheck"]
