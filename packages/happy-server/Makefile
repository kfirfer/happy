.PHONY: help start stop restart logs logs-server logs-all status build rebuild clean delete migrate shell-db shell-redis

# Default target
help:
	@echo "Happy Server - Docker Compose Commands"
	@echo ""
	@echo "Usage: make [command]"
	@echo ""
	@echo "Commands:"
	@echo "  start        Start all services"
	@echo "  stop         Stop all services"
	@echo "  restart      Restart all services"
	@echo "  status       Show status of all services"
	@echo "  build        Build images without starting"
	@echo "  rebuild      Rebuild and restart all services"
	@echo ""
	@echo "  logs         Follow logs for happy-server"
	@echo "  logs-all     Follow logs for all services"
	@echo "  logs-migrate Show migration logs"
	@echo ""
	@echo "  migrate      Run database migrations"
	@echo "  shell-db     Open PostgreSQL shell"
	@echo "  shell-redis  Open Redis CLI"
	@echo "  shell-server Open shell in happy-server container"
	@echo ""
	@echo "  clean        Stop and remove containers (keeps volumes)"
	@echo "  delete       Stop and remove containers and volumes"
	@echo ""
	@echo "  health       Check health of all services"
	@echo "  test         Test if server is responding"

# Start all services
start:
	@echo "Starting Happy Server..."
	@docker compose up -d
	@echo "Server starting at http://localhost:8003"

# Stop all services
stop:
	@echo "Stopping Happy Server..."
	@docker compose stop

# Restart all services
restart:
	@echo "Restarting Happy Server..."
	@docker compose restart

# Show status of all services
status:
	@docker compose ps -a

# Build images without starting
build:
	@echo "Building images..."
	@docker compose build

# Rebuild and restart all services
rebuild:
	@echo "Rebuilding and restarting Happy Server..."
	@docker compose up -d --build

# Follow logs for happy-server only
logs:
	@docker compose logs -f happy-server

# Follow logs for all services
logs-all:
	@docker compose logs -f

# Show migration logs
logs-migrate:
	@docker compose logs migrate

# Run database migrations
migrate:
	@echo "Running migrations..."
	@docker compose run --rm migrate

# Open PostgreSQL shell
shell-db:
	@docker compose exec postgres psql -U postgres -d handy

# Open Redis CLI
shell-redis:
	@docker compose exec redis redis-cli

# Open shell in happy-server container
shell-server:
	@docker compose exec happy-server /bin/bash

# Stop and remove containers (keeps volumes)
clean:
	@echo "Cleaning up containers..."
	@docker compose down

# Stop and remove containers and volumes
delete:
	@echo "Deleting all containers and volumes..."
	@docker compose down -v
	@echo "All data has been removed"

# Check health of all services
health:
	@echo "Checking service health..."
	@echo ""
	@echo "PostgreSQL:"
	@docker compose exec postgres pg_isready -U postgres -d handy || echo "  Not healthy"
	@echo ""
	@echo "Redis:"
	@docker compose exec redis redis-cli ping || echo "  Not healthy"
	@echo ""
	@echo "MinIO:"
	@curl -sf http://localhost:9000/minio/health/live > /dev/null && echo "  PONG" || echo "  Not healthy"
	@echo ""
	@echo "Happy Server:"
	@curl -sf http://localhost:8003/ || echo "  Not healthy"

# Test if server is responding
test:
	@echo "Testing server..."
	@curl -s http://localhost:8003/ && echo ""
	@echo ""
	@echo "Testing metrics..."
	@curl -s http://localhost:9090/metrics | head -5
