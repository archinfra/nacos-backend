FROM eclipse-temurin:17-jre-jammy

ARG NACOS_TARBALL=nacos.tar.gz

ENV MODE=standalone \
    JAVA_OPT_EXT="" \
    NACOS_HOME=/home/nacos

WORKDIR ${NACOS_HOME}

COPY ${NACOS_TARBALL} /tmp/nacos.tar.gz

RUN set -eux; \
    tar -xzf /tmp/nacos.tar.gz -C /home; \
    extracted="$(find /home -maxdepth 1 -type d -name 'nacos*' | head -n 1)"; \
    if [ -n "${extracted}" ] && [ "${extracted}" != "${NACOS_HOME}" ]; then \
      cp -a "${extracted}"/. "${NACOS_HOME}"/; \
      rm -rf "${extracted}"; \
    fi; \
    rm -f /tmp/nacos.tar.gz; \
    chmod +x ${NACOS_HOME}/bin/*.sh || true; \
    mkdir -p ${NACOS_HOME}/logs

EXPOSE 8848 9848 9849

CMD ["sh", "-c", "bin/startup.sh -m ${MODE} && tail -F logs/start.out"]
