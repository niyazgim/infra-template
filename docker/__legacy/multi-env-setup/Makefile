.PHONY: up stop rebuild logs clean

ENV ?= prod
PLATFORM ?= docker-compose
SERVICES ?= 
ENV_FILE := .env.$(ENV)

# Error messages
MSG__K3S_ERROR := "Kubernetes (k3s) support coming soon"
MSG__PLATFORM_ERROR := "Select a supported platform (docker-compose, k3s)"

# Define the command logic once at the top level
ifeq ($(PLATFORM),docker-compose)
    BASE_CMD := docker compose --profile $(ENV) --env-file $(ENV_FILE) -f docker-compose.yml
    ifeq ($(ENV),dev)
        BASE_CMD += -f docker-compose.dev.yml
    endif
    CMD_UP      := $(BASE_CMD) up -d
    CMD_STOP    := $(BASE_CMD) stop
    CMD_REBUILD := $(BASE_CMD) up -d --build $(SERVICES) --force-recreate
    CMD_LOGS    := $(BASE_CMD) logs -f
    CMD_CLEAN   := $(BASE_CMD) down -v
else ifeq ($(PLATFORM),k3s)
    # Map all actions to the error message for k3s
    CMD_UP = @echo $(MSG__K3S_ERROR)
    CMD_STOP = @echo $(MSG__K3S_ERROR)
    CMD_REBUILD = @echo $(MSG__K3S_ERROR)
    CMD_LOGS = @echo $(MSG__K3S_ERROR)
    CMD_CLEAN = @echo $(MSG__K3S_ERROR)
else
    # Fallback for unsupported platforms
    CMD_UP = @echo $(MSG__PLATFORM_ERROR); exit 1
    CMD_STOP = $(CMD_UP)
    CMD_REBUILD = $(CMD_UP)
    CMD_LOGS = $(CMD_UP)
    CMD_CLEAN = $(CMD_UP)
endif

# --- Targets ---
up:
	@echo "Starting services in $(ENV) environment..."
	$(CMD_UP)

stop:
	@echo "Stopping services in $(ENV) environment..."
	$(CMD_STOP)

rebuild:
	@echo "Rebuilding services in $(ENV) environment..."
	$(CMD_REBUILD)

logs:
	$(CMD_LOGS)

clean:
	@echo "Cleaning services in $(ENV) environment..."
	$(CMD_CLEAN)
	docker rm -f $$(docker ps -aq) 2>/dev/null || true

# =====
# Git actions
# =====
checkout-all:
ifndef BRANCH
	$(error BRANCH is undefined. Usage: make checkout-all BRANCH=main)
endif
	@echo "=== Checking out $(BRANCH) in other services ==="
	cd ../../ && git checkout $(BRANCH) || echo 
	@echo "=== Checking out $(BRANCH) on parent repo ==="
	git checkout $(BRANCH) || echo "Branch $(BRANCH) not found in parent repo"