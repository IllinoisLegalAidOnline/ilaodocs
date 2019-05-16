=============================
Rentervention
=============================

Core Configuration
-------------------

We use AWS to host the platform:

* 2 instances of EC2

  * DEV (rentervention_dev)
    * IP: 100.24.45.197
    * DNS: ec2-100-24-45-197.compute-1.amazonaws.com
  * Prod (rentervention Prod)
    * IP: 54.172.103.77
    * DNS: ec2-54-172-103-77.compute-1.amazonaws.com

* S3 Instances
  * private-rentervention holds generated files
  * public-rentervention contains content

* RDS instance: rentervention-prod

DialogFlow
Dialogflow powers the chatbot.
