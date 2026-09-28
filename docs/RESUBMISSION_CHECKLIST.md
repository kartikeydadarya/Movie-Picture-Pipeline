# Movie Picture Pipeline — Resubmission Checklist

## 1. Push to GitHub

Push the project root to a **public GitHub repository**. The repository must contain:

```text
.github/workflows/frontend-ci.yml
.github/workflows/frontend-cd.yml
.github/workflows/backend-ci.yml
.github/workflows/backend-cd.yml
```

## 2. Configure GitHub Actions secrets

Add these under **Settings → Secrets and variables → Actions**:

```text
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
AWS_SESSION_TOKEN
REACT_APP_MOVIE_API_URL
```

`REACT_APP_MOVIE_API_URL` must contain the backend LoadBalancer base URL, without `/movies`, for example:

```text
http://<backend-load-balancer-dns>
```

Never put AWS credentials in YAML or source files.

## 3. Create/verify AWS infrastructure

From `setup/terraform`:

```bash
terraform init
terraform apply
terraform output
```

The Terraform configuration creates the EKS cluster named `cluster` and the ECR repositories `frontend` and `backend`.

## 4. Verify Kubernetes access

```bash
aws eks update-kubeconfig --name cluster --region us-east-1
kubectl get nodes
```

## 5. Run the CI workflows

Create a pull request targeting `main` and verify successful runs for:

- Frontend Continuous Integration
- Backend Continuous Integration

## 6. Run the CD workflows

Merge the pull request into `main`. Verify successful runs for:

- Frontend Continuous Deployment
- Backend Continuous Deployment

Both CD workflows can also be started manually from GitHub Actions.

## 7. Obtain application URLs

Run:

```bash
kubectl get service frontend
kubectl get service backend
```

Frontend URL:

```text
http://<frontend-load-balancer-dns>
```

Backend API URL:

```text
http://<backend-load-balancer-dns>/movies
```

Open both in a browser and capture screenshots with the URL visible.

## 8. Capture reviewer evidence

```bash
kubectl get all
kubectl describe deployment frontend
kubectl describe deployment backend
```

Also capture the latest image/tag in each ECR repository.

## 9. Final review

Before submitting, confirm:

- [ ] Public GitHub repository URL is submitted.
- [ ] All four workflow files are present.
- [ ] All four workflows have a successful GitHub Actions run.
- [ ] Frontend URL is working or a screenshot is submitted.
- [ ] Backend `/movies` URL is working or a screenshot is submitted.
- [ ] `kubectl get all` screenshot is available.
- [ ] Frontend deployment screenshot is available.
- [ ] Backend deployment screenshot is available.
- [ ] Frontend ECR image details are available.
- [ ] Backend ECR image details are available.
- [ ] No AWS credentials are committed to GitHub.
