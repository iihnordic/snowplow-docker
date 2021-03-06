# Snowplow Docker

[![Build Status][travis-image]][travis]
[![License][license-image]][license]

## Introduction

This repository contains the Docker images for the following Snowplow components:

- [Scala Stream Collector][ssc]
- [Stream Enrich][se]
- [Elasticsearch Loader][es]
- [S3 Loader][s3]
- [Iglu Server][iglu-server]

They are published in the [`snowplow-docker-registry.bintray.io`][registry] docker registry.

## Pulling

You can pull the images from the registry directly:

```bash
# NSQ Scala Stream Collector image, there are others available for Kinesis, Kafka and Google PubSub
docker pull snowplow-docker-registry.bintray.io/snowplow/scala-stream-collector-nsq:0.13.0

# NSQ Stream Enrich image, there are others available for Kinesis, Kafka and Google PubSub
docker pull snowplow-docker-registry.bintray.io/snowplow/stream-enrich-nsq:0.16.0

# Elasticsearch Loader image
docker pull snowplow-docker-registry.bintray.io/snowplow/elasticsearch-loader:0.10.1

# S3 Loader image
docker pull snowplow-docker-registry.bintray.io/snowplow/s3-loader:0.6.0

# Iglu Server image
docker pull snowplow-docker-registry.bintray.io/snowplow/iglu-server:0.3.0
```

## Building

Alternatively, you can build them yourself:

```bash
# All images are based on the base image
docker pull snowplow-docker-registry.bintray.io/snowplow/base:0.1.0

# NSQ Scala Stream Collector image, there are others available for Kinesis, Kafka and Google PubSub
docker build -t snowplow/scala-stream-collector-nsq:0.13.0 scala-stream-collector/0.13.0/nsq

# NSQ Stream Enrich image, there are others available for Kinesis, Kafka and Google PubSub
docker build -t snowplow/stream-enrich-nsq:0.16.0 stream-enrich/0.16.0/nsq

# Elasticsearch Loader image
docker build -t snowplow/elasticsearch-loader:0.10.1 elasticsearch-loader/0.10.1

# S3 Loader image
docker build -t snowplow/s3-loader:0.6.0 s3-loader/0.6.0

# Iglu Server image
docker build -t snowplow/iglu-server:0.3.0 iglu-server/0.3.0
```

## Running

Create your own config file filling it according to your setup, you can find examples at the
following locations:

- [Scala Stream Collector configuration][ssc-config]
- [Stream Enrich configuration][se-config]
- [Elasticsearch Loader configuration][es-config]
- [S3 Loader configuration][s3-config]
- [Iglu Server configuration][iglu-server-config]

Next, you can run a container for each component by mounting your configuration directory:

```bash
# NSQ Scala Stream Collector container, there are others available for Kinesis, Kafka and Google PubSub
docker run \
  -v $PWD/scala-stream-collector-config:/snowplow/config \
  snowplow/scala-stream-collector-nsq:0.13.0 \ # if you have built the image
  # snowplow-docker-registry.bintray.io/snowplow/scala-stream-collector-nsq:0.13.0 if you have pulled the image
  --config /snowplow/config/config.hocon

# NSQ Stream Enrich container, there are others available for Kinesis, Kafka and Google PubSub
docker run \
  -v $PWD/stream-enrich-config:/snowplow/config \
  snowplow/stream-enrich-nsq:0.16.0 \ # if you have built the image
  # snowplow-docker-registry.bintray.io/snowplow/stream-enrich-nsq:0.16.0 if you have pulled the image
  --config /snowplow/config/config.hocon \
  --resolver file:/snowplow/config/resolver.json \
  --enrichments file:/snowplow/config/enrichments/ \
  --force-ip-lookups-download

# Elasticsearch Loader
docker run \
  -v $PWD/elasticsearch-loader-config:/snowplow/config \
  snowplow/elasticsearch-loader:0.10.1 \ # if you have built the image
  # snowplow-docker-registry.bintray.io/snowplow/elasticsearch-loader:0.10.1 if you have pulled the image
  --config /snowplow/config/config.hocon

# S3 Loader
docker run \
  -v $PWD/s3-loader-config:/snowplow/config \
  snowplow/s3-loader:0.6.0 \ # if you have built the image
  # snowplow-docker-registry.bintray.io/snowplow/s3-loader:0.6.0 if you have pulled the image
  --config /snowplow/config/config.hocon

# Iglu Server
docker run \
  -v ${PWD}/iglu-server-config:/snowplow/config \
  snowplow/iglu-server:0.3.0 \ # if you have built the image
  # snowplow-docker-registry.bintray.io/snowplow/iglu-server:0.3.0 if you have pulled the image
  --config /snowplow/config/application.conf
```

You can find more information in the readme for each image:

- [Scala Stream Collector readme][ssc-readme]
- [Stream Enrich readme][se-readme]
- [Elasticsearch Loader readme][es-readme]
- [S3 Loader readme][s3-readme]
- [Iglu Server readme][iglu-server-readme]

There is a Docker Compose example in the [example folder][example]. Iglu Server also
has a Docker Compose example in a [separate example folder][iglu-example].

## Find out more

| Technical Docs             | Setup Guide          | Roadmap & Contributing |
|----------------------------|----------------------|------------------------|
| ![i1][techdocs-image]      | ![i2][setup-image]   | ![i3][roadmap-image]   |
| [Technical Docs][techdocs] | [Setup Guide][setup] | _coming soon_          |

## Copyright and license

Copyright 2017-2018 Snowplow Analytics Ltd.

Licensed under the [Apache License, Version 2.0][license] (the "License");
you may not use this software except in compliance with the License.

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

[ssc]: https://github.com/snowplow/snowplow/tree/master/2-collectors/scala-stream-collector
[se]: https://github.com/snowplow/snowplow/tree/master/3-enrich/stream-enrich
[es]: https://github.com/snowplow/snowplow-elasticsearch-loader/
[s3]: https://github.com/snowplow/snowplow-s3-loader/
[iglu-server]: https://github.com/snowplow/iglu/tree/master/2-repositories/iglu-server

[ssc-config]: https://github.com/snowplow/snowplow/blob/master/2-collectors/scala-stream-collector/examples/config.hocon.sample
[se-config]: https://github.com/snowplow/snowplow/blob/master/3-enrich/stream-enrich/examples/config.hocon.sample
[es-config]: https://github.com/snowplow/snowplow-elasticsearch-loader/blob/master/examples/config.hocon.sample
[s3-config]: https://github.com/snowplow/snowplow-s3-loader/blob/master/examples/config.hocon.sample
[iglu-server-config]: https://github.com/snowplow/snowplow-docker/blob/master/iglu-server/example/config/application.conf

[ssc-readme]: https://github.com/snowplow/snowplow-docker/tree/master/scala-stream-collector
[se-readme]: https://github.com/snowplow/snowplow-docker/tree/master/stream-enrich
[es-readme]: https://github.com/snowplow/snowplow-docker/tree/master/elasticsearch-loader
[s3-readme]: https://github.com/snowplow/snowplow-docker/tree/master/s3-loader
[iglu-server-readme]: https://github.com/snowplow/snowplow-docker/tree/master/iglu-server

[example]: https://github.com/snowplow/snowplow-docker/tree/master/example
[iglu-example]: https://github.com/snowplow/snowplow-docker/tree/master/iglu-server/example

[registry]: https://bintray.com/snowplow/registry

[setup]: https://github.com/snowplow/snowplow/wiki/snowplow-docker-setup
[techdocs]: https://github.com/snowplow/snowplow/wiki/snowplow-docker

[techdocs-image]: https://d3i6fms1cm1j0i.cloudfront.net/github/images/techdocs.png
[setup-image]: https://d3i6fms1cm1j0i.cloudfront.net/github/images/setup.png
[roadmap-image]: https://d3i6fms1cm1j0i.cloudfront.net/github/images/roadmap.png

[travis-image]: https://travis-ci.org/snowplow/snowplow-docker.png?branch=master
[travis]: http://travis-ci.org/snowplow/snowplow-docker

[license-image]: http://img.shields.io/badge/license-Apache--2-blue.svg?style=flat
[license]: http://www.apache.org/licenses/LICENSE-2.0
