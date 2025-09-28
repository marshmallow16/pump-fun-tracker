#  Pump.fun Token Tracker

Dockerized pump.fun token tracker with automated discovery, real-time monitoring, and analytics. Built with Python, ClickHouse, and Grafana for comprehensive Solana token performance tracking.

##  Features

- **Real-time Token Discovery**: WebSocket connection to pump.fun for instant new token detection
- **Performance Monitoring**: Continuous tracking of token metrics (viewers, market cap, participants)
- **High-Performance Storage**: ClickHouse database for fast analytics on large datasets
- **Visual Dashboards**: Grafana monitoring with real-time charts and alerts
- **Containerized Architecture**: Full Docker setup with docker-compose orchestration

##  Architecture

`
        
   discovery.py      ClickHouse       poller.py     
 (finds new             (database)          (tracks         
  tokens)                                    existing)      
        
                                
                                
                       
                            Grafana      
                          (dashboard)    
                       
`

##  Quick Start

`ash
# Clone the repository
git clone https://github.com/marshmallow16/pump-fun-tracker.git
cd pump-fun-tracker

# Start all services
cd clickhouse-pumpfun
docker-compose up -d

# Access Grafana dashboard
# Open http://localhost:3000 (admin/admin)
`

##  Services

| Service | Purpose | Port | Status |
|---------|---------|------|--------|
| **discovery** | Finds new tokens via WebSocket | - |  Active |
| **poller** | Tracks existing token metrics | - |  Active |
| **clickhouse** | High-performance database | 8123 |  Healthy |
| **grafana** | Monitoring dashboards | 3000 |  Running |

##  Project Structure

`
pump-fun-tracker/
 README.md                    # Project overview (this file)
 pump-fun-king/               # Python application
    discovery.py             # Token discovery service
    poller.py                # Token polling service
    clickhouse_utils.py      # Database utilities
    common_utils.py          # Shared utilities
    Dockerfile               # Container configuration
 clickhouse-pumpfun/          # Docker infrastructure
     docker-compose.yml       # Service orchestration
     schema.sql               # Database schema
     README.md                # Docker commands reference
`

##  Development

### Prerequisites
- Docker & Docker Compose
- Git

### Setup
`ash
# Navigate to Docker setup
cd clickhouse-pumpfun

# Start infrastructure only
docker-compose up -d clickhouse grafana

# Run services manually for development
docker-compose run --rm app python discovery.py
docker-compose run --rm app python poller.py
`

### Monitoring
`ash
# Check service status
docker-compose ps

# View logs
docker-compose logs -f discovery  # Token discovery
docker-compose logs -f app        # Token polling

# Access database
docker exec -it pumpfun-clickhouse clickhouse-client
`

See [clickhouse-pumpfun/README.md](./clickhouse-pumpfun/README.md) for detailed Docker commands.

##  Data Flow

1. **Discovery Service** connects to pump.fun WebSocket
2. **New tokens** are detected and stored in ClickHouse
3. **Poller Service** tracks all active tokens in cycles
4. **Market data** (viewers, market cap, participants) collected continuously
5. **Grafana** visualizes trends and performance metrics

##  Technology Stack

- **Backend**: Python 3.11 (asyncio, aiohttp, websockets)
- **Database**: ClickHouse (time-series optimized)
- **Monitoring**: Grafana with ClickHouse datasource
- **Infrastructure**: Docker, Docker Compose
- **Networking**: Docker bridge networks with service discovery

##  Database Schema

- **tokens**: Discovered token metadata
- **token_samples**: Time-series performance data
- **token_views_1m**: Aggregated 1-minute metrics
- **polling_cycles**: Operational metadata

##  Web Interfaces

- **Grafana Dashboard**: http://localhost:3000 (admin/admin)
- **ClickHouse HTTP**: http://localhost:8123

##  Troubleshooting

### Common Issues
`ash
# Services not starting
docker-compose down && docker-compose up --build -d

# Database connection issues
docker-compose restart clickhouse

# Check container health
docker-compose ps
docker-compose logs <service-name>
`

### Development Mode
`ash
# Stop automatic services
docker-compose stop app discovery

# Keep infrastructure running
docker-compose ps  # Should show only ClickHouse and Grafana

# Run scripts manually
docker-compose run --rm app python discovery.py
`

##  License

This project is open source and available under the [MIT License](LICENSE).

##  Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test with docker-compose up -d
5. Submit a pull request

##  Acknowledgments

- **Pump.fun** for the amazing platform
- **ClickHouse** for high-performance analytics
- **Grafana** for beautiful dashboards
- **Solana** ecosystem for innovation

---

**Built with  for the Solana community**

*Track tokens like a pro! *
