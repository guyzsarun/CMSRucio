# Copyright European Organization for Nuclear Research (CERN) 2017
#
# Licensed under the Apache License, Version 2.0 (the "License");
# You may not use this file except in compliance with the License.
# You may obtain a copy of the License at http://www.apache.org/licenses/LICENSE-2.0
#
# Authors:
# - Eric Vaandering, <ewv@fnal.gov>, 2018

ARG RUCIO_VERSION
FROM rucio/rucio-daemons:release-$RUCIO_VERSION

# Add CMS policy package
RUN mkdir -p /opt/rucio/policy/CMSRucioPolicy
COPY src/policy/CMSRucioPolicy /opt/rucio/policy/CMSRucioPolicy

# Install what's needed out of dmwm/rucio/CMS branch
ADD docker/rucio-daemons/install_mail_templates.sh /tmp/
RUN /tmp/install_mail_templates.sh

# Install globus SDK
RUN python3 -m pip install --no-cache-dir globus-sdk pyyaml
ADD docker/rucio-daemons/globus-config.yml.j2 /tmp

RUN rmdir /etc/grid-security/certificates && ln -s /cvmfs/grid.cern.ch/etc/grid-security/certificates /etc/grid-security/certificates

ADD docker/rucio-daemons/cms-entrypoint.sh /

# Eric patch for cache-consumer (revisit if ever comes up again)

# ADD https://raw.githubusercontent.com/ericvaandering/rucio/fix_cache_consumer/lib/rucio/daemons/cache/consumer.py /usr/local/lib/python3.6/site-packages/rucio/daemons/cache/consumer.py
# RUN python3 -m compileall /usr/local/lib/python3.6/site-packages/rucio/daemons/cache
# ADD https://raw.githubusercontent.com/ericvaandering/rucio/fix_cache_consumer/lib/rucio/core/volatile_replica.py /usr/local/lib/python3.6/site-packages/rucio/core/volatile_replica.py
# RUN python3 -m compileall /usr/local/lib/python3.6/site-packages/rucio/core
# ADD https://raw.githubusercontent.com/ericvaandering/rucio/fix_cache_consumer/lib/rucio/common/stomp_utils.py /usr/local/lib/python3.6/site-packages/rucio/common/stomp_utils.py
# RUN python3 -m compileall /usr/local/lib/python3.6/site-packages/rucio/common
# ADD https://raw.githubusercontent.com/ericvaandering/containers/add_preparer/daemons/rucio.cfg.j2 /tmp/rucio.cfg.j2

# Cannot make patch directory unless there are patches
#RUN mkdir -p /patch

# Patch for auto approve plugin rucio/pull/6215
#ADD https://github.com/rucio/rucio/pull/6215.patch /patch/6215.patch

ENTRYPOINT ["/cms-entrypoint.sh"]
