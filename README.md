# ☁️ CloudIDE - Your Personal Cloud-Based Development Environment

CloudIDE is a powerful, self-hostable, and scalable cloud-based development environment that brings the convenience of a local editor to the cloud. Inspired by modern cloud IDEs, this project provides a seamless experience for coding, running, and managing your projects from anywhere, with a fully-featured frontend and a robust, distributed backend.

---

## ✨ Key Features

-   **💻 Fully-Featured In-Browser Editor**: A rich, VS Code-like editor experience with a file tree, syntax highlighting, and a clean user interface, built with React.
-   **⚡️ Real-Time Terminal**: An interactive terminal in the browser, giving you direct access to the underlying execution environment.
-   **🚀 Scalable and Distributed Backend**: The backend is designed with a microservices architecture, including an orchestrator to manage and distribute workloads.
-   **📦 Containerized and Isolated Environments**: Every coding session runs in a secure and isolated Docker container, ensuring a clean and predictable environment every time.
-   **☁️ Cloud-Native Deployment**: Ready for deployment on Kubernetes, with YAML configuration files included for easy setup.
-   **🌐 WebSocket Communication**: Real-time, bidirectional communication between the frontend and backend for a responsive and interactive experience.

---

## 🛠️ Tech Stack

| Component         | Technology                                                                                                                                                                                                                                                                                                                                                                                                     |
| :---------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Frontend** | ![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB) ![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)                                                                                                                                                                                              |
| **Backend** | ![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white) ![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white) ![Express.js](https://img.shields.io/badge/Express.js-000000?style=for-the-badge&logo=express&logoColor=white)                                                                       |
| **Containerization**| ![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)                                                                                                                                                                                                                                                                                                         |
| **Orchestration** | ![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)                                                                                                                                                                                                                                                                                                 |
| **Real-Time** | **WebSockets** |
| **Cloud** | **AWS S3** (for project initialization)                                                                                                                                                                                                                                                                              |

---

## 🚀 Getting Started

To get CloudIDE running on your local machine, follow these steps.

### Prerequisites

-   **Node.js** (v14 or higher)
-   **Docker**
-   **Git**

### Local Development

1.  **Clone the repository**:
    ```bash
    git clone [https://github.com/your-username/CloudIDE.git](https://github.com/your-username/CloudIDE.git)
    cd CloudIDE
    ```

2.  **Install dependencies for all services**:
    You can use the provided `run-local.sh` script to install dependencies for all services:
    ```bash
    ./run-local.sh install
    ```

3.  **Start the application**:
    The `run-local.sh` script can also start all the services in the correct order:
    ```bash
    ./run-local.sh start
    ```

4.  **Access the application**:
    Open your browser and navigate to `http://localhost:5173`. You should see the CloudIDE landing page.

---

## ☁️ Deployment

CloudIDE is designed to be deployed on a Kubernetes cluster. The `k8s` directory contains the necessary YAML files to deploy the application.

1.  **Build and push the Docker images** for each service (frontend, orchestrator-simple, runner, init-service) to a container registry.
2.  **Configure the `k8s/deployment.yaml`** file with the correct image names and any other necessary environment variables.
3.  **Apply the Kubernetes configurations**:
    ```bash
    kubectl apply -f k8s/
    ```

For more detailed deployment instructions, refer to the `DEPLOY.md` file.
