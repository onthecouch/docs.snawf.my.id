# docs.snawf.my.id

Repository for the personal documentation site [docs.snawf.my.id](https://docs.snawf.my.id).

This project uses [MkDocs](https://www.mkdocs.org/) with the [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) theme to organize notes on infrastructure, homelab services, and system administration.

## Contents

- **Homelab**: Proxmox setup, networking (AdGuard Home, Nginx Proxy Manager, Cloudflared), and media server configuration.
- **Oracle Cloud**: Instance management and Kubernetes (k3s) setup.
- **Windows**: PowerShell customization and hardware drivers.

## Local Development

Prerequisites: Python 3.x

1.  **Install dependencies**
    ```bash
    pip install -r requirements.txt
    ```

2.  **Run the development server**
    ```bash
    mkdocs serve
    ```
    The site will be accessible at `http://127.0.0.1:8000`.

3.  **Build static files**
    ```bash
    mkdocs build
    ```
    Output is generated in the `site/` directory.

## License

Copyright © 2019–2026 snawf.my.id
