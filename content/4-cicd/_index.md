---
title: "CI/CD"
weight: 4
chapter: false
pre: "<b>4. </b>"
---

## GitHub Actions pipeline

The pipeline automates testing and application delivery in this sequence:

1. Run **Unit Tests**.
2. Build the container image.
3. Push the image to **Amazon ECR**.
4. Use **YQ** to update the image tag in Helm values.
5. **Argo CD** detects the Git change and syncs it to the cluster.

## Outcome

Release steps are automated, reducing release time from around 30 minutes to **approximately 5 minutes**, based on the project results provided. Updating the tag through Git also provides a clear change history and triggers the GitOps delivery flow.
