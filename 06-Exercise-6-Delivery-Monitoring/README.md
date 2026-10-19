Exercise 6 – Real-Time Operations Monitoring and Alerting

Objective

To develop a real-time delivery monitoring and alerting system for ZAPPTTO using Python, Prometheus, Grafana, Docker, and Jenkins.

The application simulates delivery operations, exposes metrics for Prometheus monitoring, visualizes delivery performance through Grafana dashboards, and generates alerts when predefined thresholds are exceeded.

Technologies Used

* Python
* Prometheus
* Grafana
* Docker
* Jenkins
* Prometheus Python Client

Project Structure

06-Exercise-6-Delivery-Monitoring/
│
├── screenshots/
├── alert_rules.yml
├── delivery_metrics.py
├── Dockerfile
├── Jenkinsfile
├── prometheus.yml
├── requirements.txt
└── README.md

Delivery Monitoring Application

The Python application simulates delivery operations and exposes application metrics through an HTTP server running on port 8000.

The application provides the following endpoint:

* /metrics – Exposes delivery metrics in Prometheus format.

The delivery metrics are generated dynamically to simulate real-time delivery operations.

Metrics

The application exposes the following metrics:

* total_deliveries – Total number of deliveries in the current simulation.
* pending_deliveries – Number of deliveries waiting to be processed.
* on_the_way_deliveries – Number of deliveries currently in transit.
* average_delivery_time – Summary metric used to observe delivery times in seconds.

Docker Setup

Docker is used to containerize the delivery monitoring application and run the monitoring services.

The Dockerfile defines the application image, while the Jenkinsfile automates the build and deployment workflow.

1. Delivery Service

The delivery metrics application runs on:

http://localhost:8000/metrics

It continuously generates delivery metrics that can be scraped by Prometheus.

2. Prometheus

Prometheus collects metrics from the delivery application and evaluates configured alert rules.

Prometheus runs on:

http://localhost:9090

The prometheus.yml file defines the scrape targets and alert rule configuration.

3. Grafana

Grafana provides dashboards for visualizing delivery metrics and monitoring operational performance.

Grafana runs on:

http://localhost:3000

4. Jenkins

Jenkins automates the application build and deployment process using the Jenkinsfile.

Jenkins runs on:

http://localhost:8080

The pipeline includes Docker pre-checks, workspace setup, Docker image building, application startup, and monitoring service startup.

Prometheus Configuration

Prometheus is configured to scrape metrics from the following targets:

* prometheus:9090 or the configured Prometheus target.
* The delivery application on port 8000, using the host IP address or a reachable Docker service hostname, depending on the deployment configuration.

The prometheus.yml file defines the scrape configuration, while alert_rules.yml contains the alerting rules.

Running the Project

Step 1: Install Dependencies

Install the required Python packages:

pip3 install -r requirements.txt

Step 2: Run the Delivery Metrics Application

Start the application:

python3 delivery_metrics.py

Verify the metrics endpoint:

http://localhost:8000/metrics

Step 3: Start Prometheus

Run Prometheus using the configuration file:

docker run -d \
  --name prometheus \
  -p 9090:9090 \
  -v "$(pwd)/prometheus.yml:/etc/prometheus/prometheus.yml" \
  -v "$(pwd)/alert_rules.yml:/etc/prometheus/alert_rules.yml" \
  prom/prometheus

Step 4: Start Grafana

Run Grafana:

docker run -d \
  --name grafana \
  -p 3000:3000 \
  grafana/grafana

Step 5: Start Jenkins

Run Jenkins:

docker run -d \
  --name jenkins \
  -p 8080:8080 \
  -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  jenkins/jenkins:lts

Note: Ensure the required ports are available and that Prometheus can reach the application metrics endpoint. The Jenkins pipeline must also have the correct workspace paths and Docker permissions.

Verifying Prometheus

Open:

http://localhost:9090

Use the following PromQL queries to verify the delivery metrics:

total_deliveries
pending_deliveries
on_the_way_deliveries
average_delivery_time_sum / average_delivery_time_count

Prometheus should return the corresponding metric values when the application is running and the scrape target is configured correctly.

Alert Configuration

The alert_rules.yml file defines alerts to identify delivery performance issues.

High Pending Deliveries

* Metric: pending_deliveries
* Condition: Greater than 10
* Duration: 15 seconds
* Severity: warning

This alert identifies a potential backlog when pending deliveries remain above the configured threshold.

High Average Delivery Time

* Metric: Average delivery time calculated from the Summary metric.
* Condition: Greater than 30 seconds
* Severity: critical

This alert identifies delays in delivery operations when the average delivery time exceeds the configured threshold.

Verifying Targets and Alerts

Open the Prometheus targets page:

http://localhost:9090/targets

Verify that the configured targets are shown as UP.

Open the Prometheus alerts page:

http://localhost:9090/alerts

Check the alert states and verify that the configured conditions are evaluated.

Grafana Dashboard

Open:

http://localhost:3000

Configure Prometheus as a data source and create a dashboard with the following panels:

1. Total Deliveries – total_deliveries
2. Pending Deliveries – pending_deliveries
3. On-the-Way Deliveries – on_the_way_deliveries
4. Average Delivery Time – average_delivery_time_sum / average_delivery_time_count

These panels provide visibility into delivery volume, pending orders, deliveries in transit, and delivery performance.

Jenkins Pipeline

The Jenkinsfile defines the automated pipeline for the delivery monitoring application.

The pipeline consists of the following stages:

1. Pre-check Docker availability.
2. Set up the workspace and required files.
3. Build the Docker image.
4. Run the delivery metrics application.
5. Start Prometheus and Grafana.

Jenkins console output can be used to verify the execution status of each pipeline stage.

Screenshots

The screenshots/ directory can contain evidence of the following:

* Delivery metrics endpoint.
* Prometheus targets page.
* Prometheus alert states.
* Grafana dashboard.
* Jenkins pipeline execution.

Result

The real-time delivery monitoring and alerting system demonstrates how Python, Prometheus, Grafana, Docker, and Jenkins can be integrated to monitor delivery operations, visualize application metrics, identify performance issues, and automate deployment tasks.

The project provides a practical example of monitoring and alerting in a DevOps environment.