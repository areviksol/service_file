# Service File Creation Guide

## Introduction
In Linux, systemd is the standard system and service manager. This guide outlines the steps to create a systemd service file for your application, enabling easy management and automation of your application's lifecycle.

## Steps:

### 1. Create a Service File in /etc/systemd/system/

Open a text editor and create a new file with a .service extension, e.g., `flask_app.service`.

### 2. Define Service Configuration:

Inside the service file, configure your application using the following template:

```plaintext
[Unit]
Description=Gunicorn instance to serve Flask application
After=network.target

[Service]
User=ubuntu
WorkingDirectory=/etc/nginx/sites-available/flask
Environment="FLASK_APP=app.py"
ExecStart=/home/ubuntu/.local/bin/gunicorn app:app -w 4 -b 0.0.0.0:8000
Restart=always

[Install]
WantedBy=multi-user.target
```

#### Description:
- A description of your service.

#### After:
- Specifies the order in which services should be started. `network.target` ensures the network is available before starting your service.

#### User and Group:
- Specify the user and group under which your application should run. Preferably, use a non-privileged user.

#### ExecStart:
- Path to the executable of your application.

#### Restart:
- Defines the restart behavior of your service. `always` means it will be automatically restarted if it crashes.

#### WantedBy:
- Specifies which target this service should be started with.

### 3. Save and Exit:

Save the service file and exit the text editor.

### 4. Reload systemd:

After moving the service file, reload systemd to read the new service definition:

```bash
sudo systemctl daemon-reload
```

### 5. Start, Stop, Restart, or Enable the Service:

- To start the service:
```bash
sudo systemctl start flask_app
```
- To stop the service:
```bash
sudo systemctl stop flask_app
```
- To restart the service:
```bash
sudo systemctl restart flask_app
```
- To enable the service to start on boot:
```bash
sudo systemctl enable flask_app
```
- To disable the service from starting on boot:
```bash
sudo systemctl disable flask_app
```

### 6. View Service Status:

You can view the status of your service using:

```bash
sudo systemctl status flask_app
```

Created a systemd service for application that can be easily managed with start, stop, restart, and enable/disable commands.
