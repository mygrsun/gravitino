#
# Licensed to the Apache Software Foundation (ASF) under one
# or more contributor license agreements.  See the NOTICE file
# distributed with this work for additional information
# regarding copyright ownership.  The ASF licenses this file
# to you under the Apache License, Version 2.0 (the
# "License"); you may not use this file except in compliance
# with the License.  You may obtain a copy of the License at
#
#  http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing,
# software distributed under the License is distributed on an
# "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
# KIND, either express or implied.  See the License for the
# specific language governing permissions and limitations
# under the License.
#
FROM trinodb/trino:435
LABEL maintainer="dev@gravitino.apache.org"

USER root

# Only mysql, hudi, iceberg, mariadb, jmx, memory, tpch, tpcds, postgresql, hive plugin are kept
RUN rm -rf /usr/lib/trino/plugin/accumulo  \
    && rm -rf /usr/lib/trino/plugin/blackhole \
    && rm -rf /usr/lib/trino/plugin/delta-lake \
    && rm -rf /usr/lib/trino/plugin/example-http \
    && rm -rf /usr/lib/trino/plugin/geospatial \
    && rm -rf /usr/lib/trino/plugin/kafka \
    && rm -rf /usr/lib/trino/plugin/local-file \
    && rm -rf /usr/lib/trino/plugin/ml \
    && rm -rf /usr/lib/trino/plugin/mysql-event-listener \
    && rm -rf /usr/lib/trino/plugin/phoenix5 \
    && rm -rf /usr/lib/trino/plugin/prometheus \
    && rm -rf /usr/lib/trino/plugin/redshift \
    && rm -rf /usr/lib/trino/plugin/singlestore \
    && rm -rf /usr/lib/trino/plugin/thrift \
    && rm -rf /usr/lib/trino/plugin/atop \
    && rm -rf /usr/lib/trino/plugin/cassandra \
    && rm -rf /usr/lib/trino/plugin/druid \
    && rm -rf /usr/lib/trino/plugin/exchange-filesystem \
    && rm -rf /usr/lib/trino/plugin/google-sheets \
    && rm -rf /usr/lib/trino/plugin/http-event-listener \
    && rm -rf /usr/lib/trino/plugin/ignite \
    && rm -rf /usr/lib/trino/plugin/kinesis \
    && rm -rf /usr/lib/trino/plugin/mongodb \
    && rm -rf /usr/lib/trino/plugin/oracle \
    && rm -rf /usr/lib/trino/plugin/pinot \
    && rm -rf /usr/lib/trino/plugin/raptor-legacy \
    && rm -rf /usr/lib/trino/plugin/resource-group-managers \
    && rm -rf /usr/lib/trino/plugin/sqlserver \
    && rm -rf /usr/lib/trino/plugin/bigquery \
    && rm -rf /usr/lib/trino/plugin/clickhouse \
    && rm -rf /usr/lib/trino/plugin/elasticsearch \
    && rm -rf /usr/lib/trino/plugin/exchange-hdfs \
    && rm -rf /usr/lib/trino/plugin/kudu \
    && rm -rf /usr/lib/trino/plugin/password-authenticators \
    && rm -rf /usr/lib/trino/plugin/redis \
    && rm -rf /usr/lib/trino/plugin/session-property-managers \
    && rm -rf /usr/lib/trino/plugin/teradata-functions
COPY --chown=trino:trino packages/trino/conf /etc/trino
COPY --chown=trino:trino packages/gravitino-trino-connector/mysql-connector-java-8.0.27.jar /usr/lib/trino/plugin/iceberg/
COPY --chown=trino:trino packages/gravitino-trino-connector/postgresql-42.7.0.jar /usr/lib/trino/plugin/iceberg/

RUN mkdir /tmp/gravitino
COPY --chown=trino:trino packages/gravitino-trino-connector /tmp/gravitino

ARG IMAGE_NAME
ARG TAG_NAME
RUN if [ "$TAG_NAME" = "trino-*" ] ; then \
    mv /tmp/gravitino /usr/lib/trino/plugin/; \
    else echo "Copying files for other images"; \
    fi


# The following ENV are needed to update configuration file before starting Trino server, you need to
# pass these ENV variables to the container when starting it like `docker run --env GRAVITINO_HOST_IP=xxx --env GRAVITINO_HOST_PORT=xxxx`
#ENV GRAVITINO_HOST_IP
#ENV GRAVITINO_HOST_PORT
#ENV GRAVITINO_METALAKE_NAME
#ENV HADOOP_USER_NAME

ENTRYPOINT ["/bin/bash", "-c", "/etc/trino/update-trino-conf.sh && /usr/lib/trino/bin/run-trino"]
