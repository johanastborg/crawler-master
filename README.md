# Crawler Master

A Clojure-based master node for a distributed web crawling system. This application manages URL discovery, deduplication, and task distribution using RabbitMQ. It also provides a web interface for monitoring and an API for retrieving data.

## Features

- **Distributed Crawling**: Uses RabbitMQ to coordinate with crawler workers.
- **URL Management**: Tracks discovered URLs and ensures they are unique before queuing them for crawling.
- **Web Dashboard**: Simple HTML status page to view crawling metrics.
- **REST API**: JSON endpoints to retrieve discovered URLs.
- **Metrics**: Tracks crawl rate and total URL count.

## Prerequisites

- **Java Development Kit (JDK)**: Java 8 or higher.
- **Leiningen**: A build automation tool for Clojure. [Install Leiningen](https://leiningen.org/#install).
- **RabbitMQ**: A message broker server.

## Configuration

### RabbitMQ Connection

Currently, the RabbitMQ connection settings are hardcoded in `src/crawler_master/web.clj`.

**Important**: You **must** update the host address to point to your RabbitMQ server before running the application.

Open `src/crawler_master/web.clj` and find:

```clojure
(def conn (rmq/connect {:host "ec2-54-213-238-4.us-west-2.compute.amazonaws.com"}))
```

Change the host to your RabbitMQ server (e.g., `"localhost"`).

### Seed URLs

The application attempts to load initial URLs from a file at `/tmp/urls.txt`. Ensure this file exists and contains seed URLs (one per line) if you wish to seed the crawler.

## Installation

1. Clone the repository:
   ```bash
   git clone <repository-url>
   cd crawler-master
   ```

2. Install dependencies:
   ```bash
   lein deps
   ```

## Usage

To start the application:

```bash
lein run
```

The server will start on port `8080`.

## API & Interface

- **Web Dashboard**: [http://localhost:8080/html](http://localhost:8080/html)
  - Displays status, total URLs, and crawl rate.

- **Discovered URLs**: [http://localhost:8080/urls](http://localhost:8080/urls)
  - Returns a JSON list of up to 1000 discovered URLs.

## Architecture

1. **Master Node**: This application. It maintains the state of discovered URLs.
2. **RabbitMQ Exchanges**:
   - `url-crawler` (Topic Exchange)
3. **Queues & Routing**:
   - Listens to routing key `discovered-urls`.
   - Publishes to routing key `to-crawl-urls`.

## License

Copyright © 2014

Distributed under the Eclipse Public License either version 1.0 or (at your option) any later version.
