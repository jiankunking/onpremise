ARG SENTRY_IMAGE
FROM ${SENTRY_IMAGE:-getsentry/sentry:latest}

WORKDIR /usr/src/sentry

# Add WORKDIR to PYTHONPATH so local python files don't need to be installed
ENV PYTHONPATH /usr/src/sentry
COPY . /usr/src/sentry

# fix https://github.com/getsentry/sentry/issues/17157
COPY ./sentry/config.yml       /etc/sentry
COPY ./sentry/sentry.conf.py   /etc/sentry

# 覆盖server.py文件，源文件kafka地址默认本地
COPY /sentry/cover/server.py /usr/local/lib/python2.7/site-packages/sentry/conf

# Hook for installing additional plugins
RUN if [ -s requirements.txt ]; then pip install -r requirements.txt; fi

# Hook for installing a local app as an addon
RUN if [ -s setup.py ]; then pip install -e .; fi

# Hook for staging in custom configs
RUN if [ -s sentry.conf.py ]; then cp sentry.conf.py $SENTRY_CONF/; fi \
	&& if [ -s config.yml ]; then cp config.yml $SENTRY_CONF/; fi
