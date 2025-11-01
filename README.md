# Fast Tracker n8n Workflows

This repository provides a ready-to-run local environment for managing n8n workflows alongside an ngrok tunnel for receiving webhook callbacks. Drop exported workflow JSON files into the `workflows/` directory, start the stack with Docker, and the workflows will be imported automatically when the container boots.

## Getting started

1. **Copy environment configuration**
   ```bash
   cp .env.example .env
   ```
   Update `.env` with your preferred credentials. Set `NGROK_AUTHTOKEN` if you plan to expose webhooks through ngrok.

2. **Add workflows**
   Place your exported n8n workflow JSON files inside the `workflows/` directory. They will be imported on container start. The repository now ships with an example `Ensure_User.json` workflow to demonstrate the expected format—replace or extend it with your own exports.

3. **Run the stack**
   ```bash
   docker compose up --build
   ```
   The n8n editor UI will be available at [http://localhost:5678](http://localhost:5678). The default credentials are set via `.env` (initially `admin/admin`).

4. **Expose webhooks (optional)**
   When `NGROK_AUTHTOKEN` is provided, the ngrok container establishes a tunnel to the n8n instance. Visit [http://localhost:4040](http://localhost:4040) to inspect ngrok status and find the forwarded URL. Update `WEBHOOK_URL` in `.env` to match the active ngrok domain so n8n generates correct callback URLs, then restart the stack.

## Repository structure

```
.
├── docker-compose.yml    # Docker services for n8n and ngrok
├── workflows/            # Store exported n8n workflow JSON files here
├── .env.example          # Sample environment configuration
└── README.md             # Project documentation
```

## Notes

- Persistent n8n data (credentials, execution history, settings) is stored in the named Docker volume `n8n_data`.
- Workflows are imported on each container start with `--replaceExisting`, ensuring the database reflects the JSON files in this repository.
- Keep sensitive values such as ngrok auth tokens out of version control; `.env` is ignored by Git by default.
