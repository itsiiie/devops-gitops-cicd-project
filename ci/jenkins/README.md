# Jenkins CI Foundation

This phase sets up Jenkins using a controller–agent architecture.

## Key Points

- Jenkins controller runs locally in Docker
- Jenkins agent runs on Multipass (Ubuntu)
- Communication uses SSH
- All builds execute on agents, not on controller

This mirrors real-world production Jenkins architectures.
