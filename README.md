<p align="center">
<img width="500px" src="https://user-images.githubusercontent.com/16992394/147855783-07b747f3-d033-476f-9e06-96a4a88a54c6.png">
</p>
<h2 align="center"><b>Elast</b>ic Stack on <b>Docker</b></h2>
<h3 align="center">Preconfigured Security, Tools, and Self-Monitoring</h3>
<h4 align="center">Configured to be ready to be used for Log, Metrics, APM, Alerting, Machine Learning, and Security (SIEM) usecases.</h4>
<p align="center">
   <a>
      <img src="https://img.shields.io/badge/Elastic%20Stack-8.0.1-blue?style=flat&logo=elasticsearch" alt="Elastic Stack Version 7^^">
   </a>
   <a>
      <img src="https://img.shields.io/github/v/tag/sherifabdlnaby/elastdocker?label=release&amp;sort=semver">
   </a>
   <a href="https://github.com/sherifabdlnaby/elastdocker/actions/workflows/build.yml">
      <img src="https://github.com/sherifabdlnaby/elastdocker/actions/workflows/build.yml/badge.svg">
   </a>
   <a>
      <img src="https://img.shields.io/badge/Log4Shell-mitigated-brightgreen?style=flat&logo=java">
   </a>
   <a>
      <img src="https://img.shields.io/badge/contributions-welcome-brightgreen.svg?style=flat" alt="contributions welcome">
   </a>
   <a href="https://github.com/sherifabdlnaby/elastdocker/network">
      <img src="https://img.shields.io/github/forks/sherifabdlnaby/elastdocker.svg" alt="GitHub forks">
   </a>
   <a href="https://github.com/sherifabdlnaby/elastdocker/issues">
        <img src="https://img.shields.io/github/issues/sherifabdlnaby/elastdocker.svg" alt="GitHub issues">
   </a>
   <a href="https://raw.githubusercontent.com/sherifabdlnaby/elastdocker/blob/master/LICENSE">
      <img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="GitHub license">
   </a>
</p>

# Introduction
**This repo is a fork of https://github.com/sherifabdlnaby/elastdocker.**

I've updated some configurations and other stuff in order to learn how to work with ELK and how to adapt it for my requirements

Elastic Stack (**ELK**) Docker Composition, preconfigured with **Security**, **Monitoring**, and **Tools**; Up with a Single Command.


Stack Version: [8.2.0](https://www.elastic.co/blog/whats-new-elastic-8-2-0) 🎉  - Based on [Official Elastic Docker Images](https://www.docker.elastic.co/)
> You can change Elastic Stack version by setting `ELK_VERSION` in `.env` file and rebuild your images. Any version >= 8.0.0 is compatible with this template.

### My goals

I have this goals:
1. Understand logstash and how it works
2. Read a csv file, transform and index in a elastic schema
3. Read a bundle of csv files, combine, adapt, transform and index in a elastic schema

#### More points

See the original repo https://github.com/sherifabdlnaby/elastdocker


# Setup

1. Clone the Repository
     ```bash
     git clone https://github.com/GabrielSaiz/elastdocker
     ```
2. Initialize Elasticsearch Keystore and TLS Self-Signed Certificates
    ```bash
    $ make setup
    ```
    > **For Linux's docker hosts only**. By default virtual memory [is not enough](https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html) so run the next command as root `sysctl -w vm.max_map_count=262144`
3. Start Elastic Stack
    ```bash
    $ make elk           <OR>         $ docker compose up -d
    ```
4. Visit Kibana at [https://localhost:5601](https://localhost:5601) or `https://<your_public_ip>:5601`

    Default Username: `elastic`, Password: `changeme`


# Configuration

* Some Configuration are parameterized in the `.env` file.
  * `ELASTIC_PASSWORD`, user `elastic`'s password (default: `changeme` _pls_).
  * `ELK_VERSION` Elastic Stack Version (default: `8.0.1`)
  * `ELASTICSEARCH_HEAP`, how much Elasticsearch allocate from memory (default: 1GB -good for development only-)
  * `LOGSTASH_HEAP`, how much Logstash allocate from memory.
  * Other configurations which their such as cluster name, and node name, etc.
* Elasticsearch Configuration in `elasticsearch.yml` at `./elasticsearch/config`.
* Logstash Configuration in `logstash.yml` at `./elasticsearch/config/logstash.yml`.
* Logstash Pipeline in `main.conf` at `./elasticsearch/pipeline/main.conf`.
* Kibana Configuration in `kibana.yml` at `./kibana/config`.
* Rubban Configuration using Docker-Compose passed Environment Variables.

### Setting Up Keystore

You can extend the Keystore generation script by adding keys to `./setup/keystore.sh` script. (e.g Add S3 Snapshot Repository Credentials)

To Re-generate Keystore:
```
make keystore
```

### Notes


- ⚠️ Elasticsearch HTTP layer is using SSL, thus mean you need to configure your elasticsearch clients with the `CA` in `secrets/certs/ca/ca.crt`, or configure client to ignore SSL Certificate Verification (e.g `--insecure` in `curl`).

- Adding Two Extra Nodes to the cluster will make the cluster depending on them and won't start without them again.

- Makefile is a wrapper around `Docker-Compose` commands, use `make help` to know every command.

- Elasticsearch will save its data to a volume named `elasticsearch-data`

- Elasticsearch Keystore (that contains passwords and credentials) and SSL Certificate are generated in the `./secrets` directory by the setup command.

- Make sure to run `make setup` if you changed `ELASTIC_PASSWORD` and to restart the stack afterwards.

- **For Linux Users it's recommended to set the following configuration (run as `root`)**
    ```
    sysctl -w vm.max_map_count=262144
    ```
    By default, Virtual Memory [is not enough](https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html).

---------------------------

![Intro](https://user-images.githubusercontent.com/16992394/156664447-c24c49f4-4282-4d6a-81a7-10743cfa384e.png)
![Alerting](https://user-images.githubusercontent.com/16992394/156664848-d14f5e58-8f80-497d-a841-914c05a4b69c.png)
![Maps](https://user-images.githubusercontent.com/16992394/156664562-d38e11ee-b033-4b91-80bd-3a866ad65f56.png)
![ML](https://user-images.githubusercontent.com/16992394/156664695-5c1ed4a7-82f3-47a6-ab5c-b0ce41cc0fbe.png)



# License
[MIT License](https://raw.githubusercontent.com/sherifabdlnaby/elastdocker/master/LICENSE)
Copyright (c) 2020 Sherif Abdel-Naby

# Contribution

PR(s) are Open and Welcomed.
