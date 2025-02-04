FROM python:3.10-slim-buster
# Utiliser une image de base Debian
# FROM debian:buster-slim
MAINTAINER Odoo S.A. <info@odoo.com>

# Generate locale C.UTF-8 for postgres and general locale data
ENV LANG C.UTF-8

# Installer les dépendances requises
RUN apt-get update && apt-get install -y \
    git \
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

RUN groupadd -r odoo17 && useradd -r -g odoo17 odoo17

RUN su - odoo17
# Cloner la branche spécifique de votre dépôt Odoo
RUN git clone -b ${ODOO_BRANCH} https://github.com/${GITHUB_USER}/${ODOO_REPO}.git /odoo


# Se déplacer dans le répertoire Odoo
WORKDIR /odoo

COPY odoo.conf /etc/odoo/odoo.conf

RUN ls -l /etc/odoo/odoo.conf

RUN chown -R odoo17:odoo17 /etc/odoo/odoo.conf

# RUN python3 -m pip --version
# Vérifier la version de Python
RUN python3 --version

# Vérifier le chemin vers python3
RUN which python3
RUN which pip

# Installer pip pour Python 3.11, si nécessaire
RUN python3 -m ensurepip --upgrade

# Mettre à jour pip
RUN python3 -m pip install --upgrade pip

# Vérifier la version de pip
RUN python3 -m pip --version

# Installer les dépendances Python pour Odoo
RUN python3 -m pip install --no-cache-dir -r requirements.txt -vvv

# Expose Odoo services
# EXPOSE 8069 8071

# Démarrer Odoo
# CMD ["python3", "-m", "odoo"]
