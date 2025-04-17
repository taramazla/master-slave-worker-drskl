# Distributed PostgreSQL with Citus

This project implements a distributed PostgreSQL cluster using Citus extension with master-slave (primary-secondary) architecture and worker nodes. Worker nodes are configured to handle data sharding and replication, while the coordinator layer manages query routing and load balancing. All Workers are connectable to the Coordinator, and the Coordinator is connected to the Load Balancer (HAProxy). The architecture is designed for high availability and scalability.

## Architecture

This project builds a distributed PostgreSQL cluster with:

- **Coordinator Layer**: Primary and secondary coordinators
- **Worker Layer**: Three worker nodes, each with primary and secondary instances
- **Load Balancer**: HAProxy for routing database connections
- **Monitoring**: Worker node monitoring and automatic failover
- **PostGIS Integration**: Spatial data capabilities on all nodes

## Project Structure

```
/
├── docker/                    # Docker-related files
│   ├── Dockerfile             # Main Dockerfile for PostgreSQL nodes
│   └── Dockerfile.loadbalancer # Dockerfile for HAProxy load balancer
│
├── config/                    # Configuration files
│   ├── haproxy/               # HAProxy configuration
│   │   └── haproxy.cfg        # HAProxy configuration file
│   └── postgres/              # PostgreSQL configuration
│       ├── pg_hba.conf        # PostgreSQL host-based authentication config
│       └── worker-pg_hba.conf # Worker node authentication config
│
├── scripts/                   # Shell scripts
│   ├── coordinator/           # Coordinator scripts
│   │   ├── primary-entrypoint.sh   # Primary coordinator entrypoint
│   │   ├── secondary-entrypoint.sh # Secondary coordinator entrypoint
│   │   └── standby-entrypoint.sh   # Generic standby entrypoint
│   │
│   ├── worker/                # Worker scripts
│   │   ├── worker-primary-entrypoint.sh # Worker primary node entrypoint
│   │   └── worker-standby-entrypoint.sh # Worker secondary node entrypoint
│   │
│   ├── monitoring/            # Monitoring scripts
│   │   └── worker-promotion.sh      # Worker monitoring and failover script
│   │
│   ├── setup.sh               # General cluster setup script
│   └── run.sh                 # Main script to start the cluster
│
├── sql/                       # SQL files
│   └── init.sql               # Initial data and schema setup
│
├── docs/                      # Documentation
│   └── master-slave-worker.png # Architecture diagram
│
├── docker-compose.yml         # Docker Compose definition
└── requirements.txt           # Python dependencies
```

## Getting Started

To run the cluster:

```bash
./scripts/run.sh
```

This will start all the components defined in the docker-compose.yml file.