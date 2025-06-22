FROM ubuntu:24.04

ARG TARGETARCH
ENV TZ=Europe/Skopje LANG=C.UTF-8 DEBIAN_FRONTEND=noninteractive \
    NC_ADMIN=Irfan NC_PASS=ComputerEngineering

RUN apt-get update && apt-get install -y --no-install-recommends \
    curl gnupg ca-certificates tzdata sudo vim tini python3 python3-pip \
    apache2 libapache2-mod-php php-mysql php-gd php-xml php-curl php-mbstring \
    php-zip php-intl php-bz2 php-imagick php-fpm jupyter-notebook wget samba supervisor \
    && rm -rf /var/lib/apt/lists/*

ARG PROM_VERSION=2.52.0
ARG GRAFANA_VERSION=11.0.0
RUN curl -L https://github.com/prometheus/prometheus/releases/download/v${PROM_VERSION}/prometheus-${PROM_VERSION}.linux-${TARGETARCH}.tar.gz | tar -xz -C /opt && \
    ln -s /opt/prometheus-${PROM_VERSION}.linux-${TARGETARCH} /opt/prometheus && \
    curl -L https://dl.grafana.com/oss/release/grafana-${GRAFANA_VERSION}.linux-${TARGETARCH}.tar.gz | tar -xz -C /opt && \
    ln -s /opt/grafana-${GRAFANA_VERSION} /opt/grafana

COPY website/            /srv/website/
COPY chat-ui/            /srv/chat-ui/
COPY llm-chat-ui-docker/ /srv/llm-chat-ui/
COPY monitoring/         /etc/mini-cloud/monitoring/
COPY models/             /models/

RUN curl -Lo /var/www/html/nextcloud-setup.php https://download.nextcloud.com/server/installer/setup-nextcloud.php && \
    chown -R www-data:www-data /var/www/html && \
    mkdir -p /etc/supervisor/conf.d /var/log/supervisor

COPY supervisord.conf /etc/supervisor/supervisord.conf

EXPOSE 80 8080 3000 3001 8086 8888 139 445
VOLUME ["/data"]

ENTRYPOINT ["/usr/bin/tini","--"]
CMD ["supervisord","-c","/etc/supervisor/supervisord.conf"]