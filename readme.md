DevOps Internship Assignment
This project demonstrates a multi-service application orchestrated with Docker Compose, featuring a Go API, a Python Flask API, and an Nginx reverse proxy. It includes automated builds, inter-service communication, health checks, and request logging.

🚀 Getting Started
To set up and run this application locally, ensure you have Docker and Docker Compose installed on your system.

Clone this repository:
(Assuming you have cloned this repository after pushing to GitHub)

git clone https://github.com/Vishwasoratur/devops-internship-assignment.git
cd SourceCodeForDevopsAssignment

Navigate to the project root:
Ensure your terminal's current directory is SourceCodeForDevopsAssignment, which contains docker-compose.yml, nginx/, service_1/, and service_2/.

Build and Run the Services:
This command will build the Docker images for all services (Go, Python, Nginx) and start them up in detached mode. The --build flag ensures images are rebuilt if any Dockerfile or context changes.

docker-compose up --build -d

The -d flag runs the containers in detached mode, allowing you to use your terminal for other commands.

Monitor Logs (Optional):
To view the combined logs from all services, run:

docker-compose logs -f

🗺️ How Routing Works
Nginx acts as a reverse proxy, listening on http://localhost:8080 on your host machine. It routes incoming requests to the appropriate backend service based on the URL path prefix:

Go Application (Service 1):

Requests to http://localhost:8080/service1/ (e.g., /service1/hello, /service1/ping) are forwarded to the service_1 container, which listens internally on port 8001.

Endpoints:

GET /service1/hello: Returns {"message": "Hello from Service 1"}

GET /service1/ping: Returns {"status": "ok", "service": "1"}

Python Application (Service 2):

Requests to http://localhost:8080/service2/ (e.g., /service2/hello, /service2/ping) are forwarded to the service_2 container, which listens internally on port 8002.

Endpoints:

GET /service2/hello: Returns {"message": "Hello from Service 2"}

GET /service2/ping: Returns {"status": "ok", "service": "2"}

Unmatched Paths:

Any request to http://localhost:8080/ that does not match /service1/ or /service2/ will result in a 404 Not Found response from Nginx with the message "Sorry, this path is not found on the proxy."

✅ How to Test
Once the services are running (after docker-compose up -d), you can test them using curl in your terminal or directly in a web browser.

Using curl (in a new terminal window):
# Test Go application
curl http://localhost:8080/service1/hello

curl http://localhost:8080/service1/ping

# Test Python application
curl http://localhost:8080/service2/hello

curl http://localhost:8080/service2/ping

# Test unmatched path
curl http://localhost:8080/nonexistent

Using a Web Browser (e.g., Chrome):
Open your browser and navigate to:

http://localhost:8080/service1/hello

http://localhost:8080/service1/ping

http://localhost:8080/service2/hello

http://localhost:8080/service2/ping

http://localhost:8080/nonexistent

✨ Bonus Features Implemented
Logging Clarity:

Nginx is configured to log all incoming requests, including timestamps and the full request path, to /var/log/nginx/access.log (visible in docker-compose logs nginx).

Custom X-Proxied-To headers are added by Nginx for service1 and service2 locations, providing additional clarity in Nginx debug logs about which backend service handled the request.

Clean and Modular Docker Setup:

Separate Dockerfiles for each service (service_1, service_2, nginx) promote modularity and reusability.

Multi-stage builds are used for the Go application (service_1) to create small, production-ready images.

Minimal base images (python:slim-buster, alpine) are chosen to reduce image size and attack surface.

Explicit requirements.txt for Python dependencies ensures reproducible builds.

Clean build steps (e.g., rm -rf /var/lib/apt/lists/* after apt-get) minimize image bloat.

Healthcheck / Automated Test Script:

Automated Health Checks are configured in docker-compose.yml for both service_1 and service_2. These checks (curl to /ping endpoint) run periodically to confirm service availability.

The nginx service is configured with depends_on: condition: service_healthy for both backend services. This ensures that Nginx only starts after both the Go and Python applications are fully up and healthy, preventing routing errors to unready services during startup.

🧹 Cleanup
To stop and remove all running containers, networks, and images created by this project:

docker-compose down --rmi all
