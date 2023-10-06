.PHONY: default banner install install-seed seed run stop db-purge api-install env-create env db db-stop api api-stop
SHELL := /bin/bash 
default: help

help:
	make banner
	@echo "+------------------+"
	@echo "| 🏠 CORE COMMANDS |"
	@echo "+------------------+"
	@echo "make install - Setup environment and build models"
	@echo "make install-seed - Setup environment, build models and seed database"
	@echo "make seed - Seed database with dummy data"
	@echo "make run - Run database and API server"
	@echo "make stop - Stop database and API server"
	@echo "make db-purge - Delete all data in database\n"

banner:
	@echo "\n\n-------------------------------------"
	@echo "▒█▀▀█ █▀▀█ █▀▀ █▀▀█ ▒█▀▀█ ▒█▀▀█ ▀▀█▀▀"
	@echo "▒█▄▄▀ █▄▄█ ▀▀█ █▄▄█ ▒█░▄▄ ▒█▄▄█ ░▒█░░"
	@echo "▒█░▒█ ▀░░▀ ▀▀▀ ▀░░▀ ▒█▄▄█ ▒█░░░ ░▒█░░"
	@echo "+-----------------------------------+"
	@echo "|       🤖 FastAPI LLM server       |"
	@echo "+-----------------------------------+\n\n"

# ================
# 🏠 CORE COMMANDS
# ================

# ----------------------------------
# Setup environment and build models
# ----------------------------------
install:
	@make banner
	@make api-install
	@make db-stop
	@echo "✅ Installation complete. Run 'make run' to start services.\n"

# ----------------------------------------------
# Setup environment and build models and seed DB
# ----------------------------------------------
install-seed:
	@make banner
	@make api-install
	@make seed
	@make db-stop
	@echo "✅ Installation complete. Run 'make run' to start services.\n"


seed:
	@make banner
	@echo "🌱 Seeding database ..\n"
	@python3 seed.py

# ---------------------------
# Run database and API server
# ---------------------------
run:
	@make banner
	@make env
	@echo "🚀  Starting services ..\n"
	@make db
	@make api

# ----------------------------
# Stop database and API server
# ----------------------------
stop:
	@make banner
	@echo "🛑  Stopping services ..\n"
	@make db-stop
	@make api-stop

# ---------------------------
# Delete all data in database
# ---------------------------
db-purge:
	@echo "⛔  Are you sure you want to delete all data in the database? [y/N]\n"
	@read confirmation; \
	if [ "$$confirmation" = "y" ] || [ "$$confirmation" = "Y" ]; then \
		echo "Deleting generated files..."; \
		make db-stop; \
		rm -rf ../../mnt; \
		echo "Deleted."; \
	else \
		echo "Aborted."; \
	fi

# ==================
# 💁 HELPER COMMANDS
# ==================

# --------------
# Install helper
# --------------
api-install:
	@echo "🏗️  Installing services ..\n"
	@make env-create
	@make db-stop
	@make db
	@make models

# -------------------
# Create a virtualenv
# -------------------
env-create:
	@echo "Creating virtual environment .."
	@if [ -d "./venv" ]; then \
		echo "Environment and dependecies created already, loading .."; \
		source ./venv/bin/activate; \
	else \
		echo "Install dependencies .."; \
		python3 -m venv venv; \
		. venv/bin/activate; \
		pip3 install -r requirements.txt; \
	fi

# ---------------------------
# Load an existing virtualenv
# ---------------------------
env:
	@echo "loading virtual environment if exists"
	@if [ -d "./venv" ]; then \
		source ./venv/bin/activate; \
	fi

# -----------------
# Start Postgres DB
# -----------------
db:
	@echo "Starting Postgres with pgvector .."
	@cd ../../ && docker-compose -f docker-compose.yml up -d db

# -----------------
# Stop Postgres DB
# -----------------
db-stop:
	@cd ../../ && docker-compose -f docker-compose.yml up -d db

# ------------
# Build models
# ------------
models:
	@echo "💽 Building models in database .."
	@sleep 60 && python3 models.py

# -----------------
# Start FastAPI API
# -----------------
api:
	@echo "⚡ Starting FastAPI API server .."
	@sleep 5 && uvicorn main:app --port 8888 --reload

# -----------------
# Stop FastAPI API
# -----------------
api-stop:
	@echo "🛑 Stopping FastAPI server .."
	@killall uvicorn
