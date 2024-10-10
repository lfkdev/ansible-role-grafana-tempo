# Grafana Tempo Role

Installs and configures [Grafana Tempo](https://grafana.com/oss/tempo/).

## Requirements
- Ansible >= 2.9
- Supported platforms:
  - Debian family
  - RedHat family (not tested)

## Role Variables
Available variables are listed below, along with default values (see `defaults/main.yml`):

```yaml
tempo_version: "2.6.0"
```
The version of Grafana Tempo to install. If you change this, make sure to update the checksums accordingly.

```yaml
tempo_os_arch: "linux_amd64"
```
The operating system and architecture for the Tempo binaries.

```yaml
tempo_package_deb: "tempo_{{ tempo_version }}_{{ tempo_os_arch }}.deb"
```
The name of the .deb package file for Tempo.

```yaml
tempo_download_url_deb: "https://github.com/grafana/tempo/releases/download/v{{ tempo_version }}/{{ tempo_package_deb }}"
```
The URL from which to download the Tempo package for Debian-based systems.

```yaml
tempo_install_dir: "/etc/tempo"
```
The directory where Tempo configuration files will be stored.

```yaml
tempo_config_file: "config.yml"
```
The name of the Tempo configuration file.

```yaml
tempo_user: "tempo"
```
The system user and group under which Tempo will run.

```yaml
tempo_service_name: "tempo"
```
The name of the Tempo service.

```yaml
tempo_listening_port: 3200
```
The port on which Tempo will listen for incoming HTTP requests.

```yaml
tempo_storage_s3_endpoint: "s3.us-east-1.amazonaws.com"
tempo_storage_s3_bucket: "grafana-traces-data"
tempo_storage_s3_access_key: ""
tempo_storage_s3_secret_key: ""
tempo_storage_s3_insecure: true
```
Configuration for S3-compatible storage backend. Set tempo_storage_s3_insecure to false if the endpoint uses HTTPS.

```yaml
tempo_wal_path: "/var/tempo/wal"
tempo_local_path: "/var/tempo/blocks"
```
Paths for the Write-Ahead Log (WAL) and local block storage.

```yaml
tempo_block_retention: "48h"
```
The retention period for blocks in the compactor.

```yaml
tempo_enable_metrics_generator: true
tempo_external_labels:
  - source: "tempo"
  - cluster: "linux-microservices"
tempo_remote_write:
  - url: "http://localhost:9090/api/v1/write"
    send_exemplars: true
tempo_remote_write_url: "http://localhost:9090/api/v1/write"
```
Configuration for enabling the metrics generator, external labels, and remote write endpoints.

```yaml
tempo_receivers:
  otlp:
    protocols:
      grpc:
      http:
  jaeger:
    protocols:
      thrift_http:
      grpc:
      thrift_binary:
      thrift_compact:
  zipkin:
  opencensus:
  kafka:
```
Configure the receivers that Tempo will use to ingest tracing data.

## Example Playbook
```yaml
- hosts: myorg_monitoring
  roles:
    - role: grafana_tempo
  tags:
    - tempo
```

## License
MIT