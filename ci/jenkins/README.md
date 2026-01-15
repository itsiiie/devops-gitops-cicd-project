## Jenkinsfile-Based Pipeline

CI logic is stored inside the repository using a Jenkinsfile.

Benefits:

- Version-controlled pipeline
- Reviewed through Git
- Reproducible CI behavior
- Jenkins remains stateless

The pipeline currently performs:

- Source checkout
- Basic CI metadata logging
- Sanity checks
