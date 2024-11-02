FROM debian:bookworm-slim
MAINTAINER Odoo S.A. <info@odoo.com>

# Generate locale C.UTF-8 for postgres and general locale data
ENV LANG C.UTF-8

# Installer les dépendances requises
RUN apt-get update && apt-get install -y \
    git \
    python3 \
    python3-pip \
    python3-dev \
    python3-num2words \
    python3-phonenumbers \
    python3-pyldap \
    python3-qrcode \
    python3-renderpm \
    python3-setuptools \
    python3-slugify \
    python3-vobject \
    python3-watchdog \
    python3-xlrd \
    python3-xlwt \
    libxml2-dev \
    libxslt1-dev \
    zlib1g-dev \
    libsasl2-dev \
    libldap2-dev \
    libssl-dev \
    libffi-dev \
    build-essential \
    libjpeg-dev \
    libpq-dev \
    && rm -rf /var/lib/apt/lists/*

# install latest postgresql-client
# RUN set -x \
# echo "deb http://apt.postgresql.org/pub/repos/apt/ jammy-pgdg main" > /etc/apt/sources.list.d/pgdg.list \
# && curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc | tee /etc/apt/trusted.gpg.d/pgdg.asc \
# && rm -rf /var/lib/apt/lists/* \
# && apt-get update \
# && apt-get install -y postgresql-client \
# && rm -rf /var/lib/apt/lists/*

# Mettre à jour pip pour éviter les erreurs de compatibilité
RUN pip3 install --upgrade pip setuptools wheel

# Install Odoo
ENV ODOO_VERSION 17.0
ARG GITHUB_USER=ultrthierry
ARG ODOO_REPO=odoo-17
ARG ODOO_BRANCH=main

# Cloner la branche spécifique de votre dépôt Odoo
RUN git clone -b ${ODOO_BRANCH} https://github.com/${GITHUB_USER}/${ODOO_REPO}.git /odoo

# Se déplacer dans le répertoire Odoo
WORKDIR /odoo

# Installer les dépendances Python pour Odoo
RUN pip3 install -r requirements.txt

# Expose Odoo services
EXPOSE 8069 8071

# Démarrer Odoo
CMD ["python3", "-m", "odoo"]
