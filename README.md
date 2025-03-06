# Local AI 

**Local AI** is an open, Docker Compose template designed to quickly bootstrap
a robust, low/no code development environment comprising AI, workflow, and storage
modules.

This package has been personalized to fit my needs. See the "Important Notes" section
before the installation guide to determine if/what changes would be appropriate for 
your installation.

## What's In The Package

- **Ollama** A cross-platform LLM platform to install and run the latest local LLMs
|[Website](https://ollama.com/), [Github](https://github.com/ollama/ollama)
- **Supabase** is an open source database as a service that leverages vectoring ideal for RAG agents
|[Website](https://supabase.com/), [Github](https://github.com/supabase/supabase)
- **Qdrant**, like Supabase, is an open source vector database store. Qdrant may outperform Supabase in some instances
|[Website](https://qdrant.tech/), [Github](https://github.com/qdrant/qdrant)
- **n8n** is a low-code platform with over 400 integrations and advanced AI components
|[Website](https://n8n.io/), [Github](https://github.com/n8nio)
- **FlowiseAI** A low/no code AI agent builder that pairs very well with n8n
|[Website](https://flowiseai.com/), [Github](https://github.com/flowiseai/flowise)
- **Open WebUI** is a ChatGPT-like interface to privately interact with your local models and n8n agents
|[Website](https://openwebui.com/), [Github](https://github.com/open-webui/open-webui)


## Prerequisites

- [Git/GitHub Desktop](https://desktop.github.com/) - For easy repository management
- [Docker/Docker Desktop](https://www.docker.com/products/docker-desktop/) - Required to run all services
- If using a non-headless OS, the desktop version of these is recommended to monitor this many containers.
- Designed to work with on Windows/Mac/Linux. Confirmed to work on Windows 11 and Ubuntu Live Server 24.04.2

## Important Notes

**Local AI** is configured for my specific architecture. Take note of the following:
- This has configuration for vector db to be stored on a mounted secondary disk
- **REQUIRED** update: in docker-compose.yml, search '/vector-store/' for two instances.
1. If you intend to use a secondary disk as well, replace '/vector-store/' with the path to your storage folder. Keep everything after.
2. If you intend to use the standard storage, replace '/vector-store/' with './'. Keep everything after that.

- **Optional** config: If you do not plan to use any of these modules (e.g., will use Supabase only, omit Qdrant), you can find
those modules' section in the docker-compose.yml under 'services' and comment them out with a '#' to save storage space.
- For Supabase, all sub containers are required in order to function. They are [studio, kong, auth, rest, realtime, storage, imgproxy, meta, function, analytics, db, vector, and supavisor]. Do not comment these out.
- The default Ollama image is CPU-Only. See the [Ollama documentation](https://ollama.com/) to determine the correct image if you wish you utilize your GPU.
- In the docker-compose.yml, the x-init-ollama service defines what model(s) to pull on first start. You can change the portion of the command 'LAMA_HOST=ollama:11434 ollama pull {your model of choice}' For multiple models, include the whole command for each, separated by a semicolon.

## Installation

1. Clone the repo and navigate to the project directory
```bash
git clone https://github.com/theDrewski81/local-ai.git
cd local-ai
```

**Before running compose, there are configuration changes that must be made**

2. Copy '.env.example' and rename as '.env'.
3. In .env, set the following environment variables. The [Supabase Self-Hosting Doc](https://supabase.com/docs/guides/self-hosting/docker)
has a **Generate API** tool for the Supabase JWT_SECRET, ANON_KEY, and SERVICE_ROLE_KEY. For the rest, create your own. POOLER_TENANT_ID 
can be any 4-5 digit number.
4. Make updates to '/docker/docker-compose.yml' per the previous section.
5. Execute with 
```
docker compose up -d
```
6. Access the front end of these containers through a web browser.
If running on a server, replace 'localhost' with the server IP address
- **Supabase** http://localhost:8000
- **Supabase Analytics** http://localhost:4000
- **Ollama** http://localhost:11434
- **n8n** http://localhost:5678
- **Qdrant** http://qdrant:6333
- **Open WebUI** http://localhost:3000
- **Flowise** http://localhost:3001
