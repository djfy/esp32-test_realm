# esp32-template
This is a template for running ESP-IDF v5.3.1 (using Python 3.10.12) in a VSCode devcontainer.

# Pre-requisites
- Docker/Docker Desktop
  - Use WSL2 backend for Docker on Windows
- Follow the `usbipd` setup https://github.com/espressif/vscode-esp-idf-extension/blob/608a45e2e0a2069b3f954c97f9eff65e96b8ff7d/docs/tutorial/using-docker-container.md#usbipd

# Usage

- Clone repo
- Open in VSCode devcontainer
- Use ESP-IDF VSCode extension to generate a project in the `projects` directory or wherever you see fit. This will generate the project files (e.g., `./projects/hello_world`) and re-open VSCode in that project. Instead, re-open the previous repository-level workspace and then delete the project's `.devcontainer` (e.g., delete `./projects/hello_world/.devcontainer`).
- Use `VSCode > File > Add Folder to Workspace` to add the newly generated project folder to the VSCode workspace (e.g., `./projects/hello_world`) .
  - For example, this modifies the root `.code-workspace` file like this:
    ```json
    {
      "folders": [
        {
          "path": "."
        },
        {
          "path": "projects/hello_world" // ⚠️ You just added this ⚠️
        }
      ],
      "settings": {}
    }
    ```
- <mark>**IMPORTANT**</mark>: Target the project with the VSCode ESP-IDF extensions using `Command Palette (ctrl + shift + P) > ESP-IDF: Pick a Workspace Folder`
- Use ESP-IDF VScode extension to set ESP-IDF VSCode configs for this project:
  - Plugin commands:
    - `Select Serial Port` (see this `usbipd` setup to allow this in the devcontainer https://github.com/espressif/vscode-esp-idf-extension/blob/608a45e2e0a2069b3f954c97f9eff65e96b8ff7d/docs/tutorial/using-docker-container.md#usbipd)
      - For example, the drop-down options will include something like `/dev/ttyUSB0 ESP32-S3 (QFN56) (revision v0.2)`; choose the one that best matches your hardware configuration.
    - `Set Espressif Target (IDF_TARGET)`
      - For example, the dropd-own options will include a list of all the ESP32 chips; choose the one that best matches your hardware configuration.
  - Example `./projects/hello_world/.vscode/settings` result:
    ```json
    {
      "idf.adapterTargetName": "esp32s3",
      "idf.openOcdConfigs": [
          "interface/ftdi/esp32_devkitj_v1.cfg",
          "target/esp32s3.cfg"
      ],
      "idf.port": "/dev/ttyUSB0"
    }
    ```
- Congrats! You should now be able to build the project and flash it using the ESP-IDF extension!

# `usbipd` quick start setup
Follow: https://github.com/espressif/vscode-esp-idf-extension/blob/608a45e2e0a2069b3f954c97f9eff65e96b8ff7d/docs/tutorial/using-docker-container.md#usbipd
- `usbipd list`
  - `usbipd bind --busid <BUSID>`
    - This command needs to be used only one time,unless the computer has restarted
  - `usbipd attach --wsl --busid <BUSID>`
  - `dmesg | tail`

# Issues
- Seems there's ongoing issues with this extension and proper project setup, hence why we don't want to use the ESP-IDF extension's auto-generated `.devcontainer`:
    - https://github.com/espressif/vscode-esp-idf-extension/pull/1282/files?diff=split&w=0
    - https://github.com/espressif/vscode-esp-idf-extension/issues/1320