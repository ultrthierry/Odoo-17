FROM debian:buster-slim
MAINTAINER Odoo S.A. <info@odoo.com>

# Generate locale C.UTF-8 for postgres and general locale data
ENV LANG C.UTF-8

# Install some deps, lessc and less-plugin-clean-css, and wkhtmltopdf
RUN set -x; \
        apt-get update \
        && apt-get install -y --no-install-recommends \
            ca-certificates \
            curl \
            dirmngr \
            fonts-noto-cjk \
            gnupg \
            libssl-dev \
            node-less \
            npm \
            python3-num2words \
            python3-pip \
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
            xz-utils \
        && curl -o wkhtmltox.deb -sSL https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.5/wkhtmltox_0.12.5-1.stretch_amd64.deb \
        && echo '7e35a63f9db14f93ec7feeb0fce76b30c08f2057 wkhtmltox.deb' | sha1sum -c - \
        && apt-get install -y --no-install-recommends ./wkhtmltox.deb \
        && rm -rf /var/lib/apt/lists/* wkhtmltox.deb

# install latest postgresql-client
RUN set -x \
        echo "deb http://apt.postgresql.org/pub/repos/apt/ jammy-pgdg main" > /etc/apt/sources.list.d/pgdg.list \
        && curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc | tee /etc/apt/trusted.gpg.d/pgdg.asc \
        && rm -rf /var/lib/apt/lists/* \
        && apt-get update \
        && apt-get install -y postgresql-client \
        && rm -rf /var/lib/apt/lists/*

# Install rtlcss (on Debian buster)
RUN set -x; \
    npm install -g rtlcss

# Install and build Odoo dependencies
ARG ODOO_VERSION=main
ARG ODOO_SOURCE=odoo-17
ADD https://github.com/ultrthierry/$ODOO_SOURCE/$ODOO_VERSION/requirements.txt /build/odoo/requirements.txt
COPY --chmod=700 build/ /build/
RUN apt-get -qq update \
    && xargs -a /build/install/${DISTRIBUTION}/apt-build-deps.txt apt-get install -yqq --no-install-recommends \
    # Python Packages
    && pip install --no-cache-dir \
        --requirement /build/odoo/requirements.txt \
        --requirement /build/requirements.txt \
        --requirement /build/extra-requirements.txt \
        --requirement /build/test-requirements.txt \
        --constraint /build/odoo/requirements.txt \
    && (python3 -m compileall -q /usr/local/lib/python3*/ || true) \
    # Cleanup
    && xargs -a /build/install/${DISTRIBUTION}/apt-build-deps.txt apt-get purge -yqq \
    && apt-get autopurge -yqq \
    && rm -rf /var/lib/apt/lists/* /tmp/* \
    && rm -rf /build \
    && sync

# Mettre à jour pip pour éviter les erreurs de compatibilité
RUN pip3 install --upgrade pip
# Copy entrypoint script and Odoo configuration file
RUN pip3 install paramiko
COPY ./entrypoint.sh /
COPY ./odoo.conf /etc/odoo/

# Set permissions and Mount /var/lib/odoo to allow restoring filestore and /mnt/extra-addons for users addons
RUN chown odoo /etc/odoo/odoo.conf \
    && mkdir -p /mnt/extra-addons \
    && chown -R odoo /mnt/extra-addons
VOLUME ["/var/lib/odoo", "/mnt/extra-addons"]

# Expose Odoo services
EXPOSE 8069 8071

# Set the default config file
ENV ODOO_RC /etc/odoo/odoo.conf

COPY wait-for-psql.py /usr/local/bin/wait-for-psql.py

# Set default user when running the container
USER odoo

ENTRYPOINT ["/entrypoint.sh"]
CMD ["odoo"]
