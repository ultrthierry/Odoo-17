FROM python:3.11-slim-buster
MAINTAINER Odoo S.A. <info@odoo.com>

# Generate locale C.UTF-8 for postgres and general locale data
ENV LANG C.UTF-8

# Installer les dépendances requises
RUN apt-get update && apt-get install -y \
    git \
    python3-pip \
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
# RUN pip3 install --upgrade pip setuptools wheel

# RUN pip3 install --upgrade "pip<21.0"

# Mettre à jour pip et setuptools
RUN python3 -m pip install --upgrade pip setuptools

# Assurez-vous que `python` et `python3` pointent vers Python 3.10
# RUN ln -sf /usr/local/bin/python3.10 /usr/local/bin/python
RUN ln -sf /usr/local/bin/python3.10 /usr/local/bin/python3

# Install Odoo
ENV ODOO_VERSION 17.0
ARG GITHUB_USER=ultrthierry
ARG ODOO_REPO=odoo-17
ARG ODOO_BRANCH=main

# Cloner la branche spécifique de votre dépôt Odoo
RUN git clone -b ${ODOO_BRANCH} https://github.com/${GITHUB_USER}/${ODOO_REPO}.git /odoo


# Se déplacer dans le répertoire Odoo
WORKDIR /odoo

COPY odoo.conf /etc/odoo/odoo.conf

RUN python3 -m pip --version

# Installer les dépendances Python pour Odoo
RUN python3 -m pip install --no-cache-dir -r requirements.txt -vvv

# Expose Odoo services
# EXPOSE 8069 8071

# Démarrer Odoo
# CMD ["python3", "-m", "odoo"]
