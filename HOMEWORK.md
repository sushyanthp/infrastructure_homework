# Infrastructure Homework

# Client Requirement:
* Provision a highly available and scalable Wordpress app on GCP for POC

# Business Requirements:
* Avoid downtime and cost is not a factor
* Low complexity of infrastructure management 

# Technical Requirements:
* Minimize latency to customers in SF and Tokyo since majority of users from these locations
* Scale quickly based on traffic
* Static website with low DB usage
* Cloud native & best practices GCP infrastructure is preferred

# Proposed Solution - Cloud Run with HTTP(S) LB using serverless Network Endpoint Groups (NEGs)
* Create a custom VPC with subnets in the us-west2 and asia-northeast1 regions as a best practice
* Create a Cloud Firestore database with multi region replication. Ensure app is configured to access this database.
* Make sure the custom WordPress app is containerized with its image checked into GCR. For future updates to the application, we can use Cloud Build to push updates
* Create a Cloud Run service using the application's container image checked into GCR in both the us-west2 and asia-northeast1 regions
* Create a Global HTTP(S) load balancer using serverless NEGs (as outlined in the images). Complete details in [2]
  * Setup the backend service by creating a serverless NEG in the us-west2 and asia-northeast1 regions
  * Ensure an SSL certificate is applied on the HTTPS load balancer
  * Enable CDN on the HTTPS load balancer for caching purposes
  * Enable Cloud Armor to protect against DDoS attacks
* Inspect Cloud Run service logs (request and container logs) in Cloud Logging
* Monitoring for Cloud Run is automatically integrated to Cloud Monitoring

# References
[1] https://cloud.google.com/blog/products/networking/better-load-balancing-for-app-engine-cloud-run-and-functions
[2] https://cloud.google.com/load-balancing/docs/negs/setting-up-serverless-negs
[3] https://cloud.google.com/appengine/docs/the-appengine-environments
[4] https://gcp.solutions/diagram/websites-gae-microservices
