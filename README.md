# OpenSearch - Docker - Compose

## Key concepts

- OpenSearch is [the successor of OpenDistro](https://opendistro.github.io/for-elasticsearch/blog/2021/06/forward-to-opensearch/)
- OpenSearch = Elasticsearch
- OpenSearch Dashboards = Kibana
- Logstash = Logstash oss with opensearch output plugin

## Cluster setup

- Raise your host's ulimits for ElasticSearch to handle high I/O :

```bash
sudo sysctl -w vm.max_map_count=512000
# Persist this setting in `/etc/sysctl.conf` and execute `sysctl -p`
```

- Now, we will generate the certificates for the cluster :

# You may want to edit the OPENDISTRO_DN variable first
```bash
bash generate-certs.sh
```

- Start the cluster :

```bash
docker-compose up -d
```

- Wait about 60 seconds and run `securityadmin` to initialize the security plugin :

```bash
sudo docker compose exec os01 bash -c "chmod +x plugins/opensearch-security/tools/securityadmin.sh && bash plugins/opensearch-security/tools/securityadmin.sh -cd config/opensearch-security -icl -nhnv -cacert config/certificates/ca/ca.pem -cert config/certificates/ca/admin.pem -key config/certificates/ca/admin.key -h localhost"
```
