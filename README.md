# PLUGIN Healthcare docs

This repository contains the source code of the PLUGIN documentation websites. It is currently under development.

## Branches

- `main` branch is published on GitHub pages
- `dev` branch is used for writing new content and draft documents

> [!IMPORTANT]
> Zensical doesn't yet support [draft documents](https://zensical.org/docs/setup/basics/?h=draft#unsupported-settings). So until such time we use the `dev` branch.

## Quick start

1. Clone the repository

   ```bash
   git clone https://github.com/plugin-healthcare/handbook.git
    cd handbook
    ```

2. Install dependencies

    ```bash
    uv sync
    ```

3. Activate the virtual environment
    - a. linux/macOS

        ```bash
        source .venv/bin/activate
        ```

    - b. Windows

        ```bash
        .venv\Scripts\activate
        ```

4. Serve the documentation locally

    ```bash
    zensical serve
    ```

    > open your browser and navigate to `http://localhost:8000`.
