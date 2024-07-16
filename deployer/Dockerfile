FROM marketplace.gcr.io/google/debian12 AS build

RUN apt-get update \
    && apt-get install -y --no-install-recommends gettext

ADD dwn-server /tmp/chart
RUN cd /tmp \
    && tar -czvf /tmp/dwn-server.tar.gz chart
ADD apptest/deployer/dwn-server /tmp/test/chart
RUN cd /tmp/test \
    && tar -czvf /tmp/test/dwn-server.tar.gz chart/

ADD schema.yaml /tmp/schema.yaml
ADD apptest/deployer/schema.yaml /tmp/apptest/schema.yaml

FROM gcr.io/cloud-marketplace-tools/k8s/deployer_helm
COPY --from=build /tmp/dwn-server.tar.gz /data/chart/
COPY --from=build /tmp/test/dwn-server.tar.gz /data-test/chart/
COPY apptest/deployer/schema.yaml /data-test/
COPY --from=build /tmp/apptest/schema.yaml /data-test/
COPY --from=build /tmp/schema.yaml /data/
