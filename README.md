# 🚀 ERPNext 15 Deployment & Management (Docker + Ansible)

This project automates the deployment of **ERPNext v15** and related Frappe-based applications (e.g. **HRMS**, **Payments**) using **Docker Compose** and **Ansible**. It enables consistent, reliable, and repeatable deployments across environments while reducing manual effort and the risk of human error.

---

## 📦 Repository Overview

### `app.json`

Defines the applications to be included in the deployment. Each app is listed with its GitHub repository URL and the desired branch (typically `version-15`).

Example:

```json
[
  {
    "url": "https://github.com/frappe/erpnext.git",
    "branch": "version-15"
  },
  {
    "url": "https://github.com/frappe/hrms.git",
    "branch": "version-15"
  },
  {
    "url": "https://github.com/frappe/payments.git",
    "branch": "version-15"
  }
]
```

You can add or remove apps here before building the Docker image.

playbook.yml (Ansible)

Performs the following tasks on the target server:
1. Installs Docker and Docker Compose
2. Creates the deployment directory (/opt/erpnext15 by default)
3. Copies Docker files to the server
4. Starts the ERPNext services using Docker Compose

Run the playbook:
`ansible-playbook -i hosts playbook.yml`

Make sure the hosts inventory file includes the IP/hostname of your target machine.

## 🐳 Docker Setup

docker-compose.yml

Defines the full ERPNext ecosystem with the following services:
	•	backend – Bench backend (Python + Frappe)
	•	frontend – Nginx reverse proxy
	•	db – MariaDB (main database)
	•	redis-queue / redis-cache – Redis instances
	•	create-site / configurator – Site initialization
	•	scheduler / worker / websocket – Background job handling

## 📥 Add or Remove Apps

### ✅ Add a New App
1.	Edit app.json:

 ```json
{
  "url": "https://github.com/example/custom_app.git",
  "branch": "main"
}
```

2.	Rebuild Docker image:
 `docker build -t your_image_name:v15 .`


3.	Update docker-compose.yml with the new image.
4.	Add install command to the create-site service:
 `bench --site frontend install-app custom_app`

5.	Re-deploy:
  ```
docker-compose down
docker-compose up -d
```

## ❌ Remove an App
1.	Delete the app from app.json.
2.	Remove the install-app line in the create-site container.
3.	Rebuild the Docker image.
4.	Re-deploy containers.

## 🔁 Workflow Summary
1.	Modify app.json with the apps you want.
2.	Build the Docker image with those apps.
3.	Update docker-compose.yml to reflect any image or service changes.
4.	Use Ansible to deploy:
	• Installs Docker
	• Sets up the environment
	• Starts the containerized ERPNext system

## 🌐 Access

Your ERPNext site will be available at:
`http://<server-ip>:8080`

## 📎 Notes
•	Ensure ports 8080, 80, and 443 are open on your server.
•	Use volumes to persist database and site data.
•	Consider SSL via Nginx or external proxies for production.

## 🛠 Requirements
•	Ansible (>= 2.9)
•	Docker & Docker Compose
•	Git


## 🧾 License

MIT



