# DECT Messaging Demonstrator — Brief

Lightweight server + web viewer for Snom Multicell DECT base stations (M400, M900).

Supports:
- SMS/text messages
- Alarms and event notifications
- Beacon/heartbeat messages for presence/status
- UDP-based reception from base stations (default port 10300)

Quick start:
- Run the automated script: `source runAll.sh` (Docker-based)
- Web viewer: http://localhost:8081
- Server listens on UDP 10300 by default

Run manually: use the provided scripts in `app/` (DECTMessagingServer.py, DECTMessagingViewer.py) or build/run the Docker image.

See README.md for full installation and configuration details.

# DECT Messaging Demonstrator INSTALL

A simple message server for Snom Multicell DECT M400 and M900 systems.

This application provides a simple server to receive and display messages/alarms and beacons from Snom DECT devices.

## Quick Start (Automated)

The easiest way to get started is to use the provided script, which builds and runs the entire application within a Docker container.

From the root of the project, run:
```bash
source runAll.sh
```

Once running, you can access the web viewer at [http://localhost:8081/](http://localhost:8081/).

## Manual Installation and Usage

If you prefer to set up and run the application step-by-step, follow the instructions below.

### Prerequisites

*   Git
*   Docker (recommended) or Python 3 with `pip`

### 1. Get the Code

Clone the repository and initialize the necessary submodules.

```bash
# Clone the repository
git clone https://github.com/snom-project/DECTMessagingDemonstrator.git --config core.autocrlf=input

# Navigate into the project directory
cd DECTMessagingDemonstrator

# Initialize and update submodules
git submodule update --init --recursive
```

> **Note for Windows Users:** Using `--config core.autocrlf=input` is important to ensure files maintain Unix-style line endings, which is necessary for compatibility within the Docker environment.

### 2. Running the Application

You can run the application using Docker (the recommended method) or directly with Python.

#### Option A: Run with Docker (Recommended)

1.  **Build the Docker image:**
    ```bash
    docker build -t snommd .
    ```

2.  **Run the Docker container:**
    This command starts the container in detached mode, maps the required ports, and ensures it restarts automatically.
    ```bash
    sudo docker run -dit --restart unless-stopped -p 10300:10300/udp -p 8081:8081 snommd
    ```

The application is now running. You can access the viewer at [http://localhost:8081/](http://localhost:8081/).

#### Option B: Run without Docker

If you are not using Docker, you can run the components directly on your local machine.

1.  **Install Python Dependencies:**
    The `install_packages.sh` script will set up a virtual environment and install all required packages from `requirements.txt`.
    ```bash
    source install_packages.sh
    ```

2.  **Start the Viewer:**
    The viewer is a web interface that runs on port 8081.
    ```bash
    # Navigate to the app directory
    cd app
    # Start the viewer
    python DECTMessagingViewer.py
    ```
    You can check if it's running by opening [http://localhost:8081/](http://localhost:8081/) in your browser.

3.  **Start the Messaging Server:**
    In a separate terminal, start the core messaging server, which listens for DECT messages on UDP port 10300.
    ```bash
    # Navigate to the app directory
    cd app
    # Start the server
    python DECTMessagingServer.py 10300
    ```

## Base Station Configuration

For the Snom Multicell base station to communicate with the server, you must configure it to send messages to the IP address of the machine running this application, using the correct port (10300/UDP).
Use Menu Alarm for firmware > 790.X or Management for older firmware

