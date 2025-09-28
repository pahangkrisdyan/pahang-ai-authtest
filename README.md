# Pahang AI Auth Test

A Keycloak OIDC authentication example served with nginx in Docker.

## Docker Hub Integration

This repository is configured to automatically build and push Docker images to Docker Hub at `pahangkrisdyan/pahang-ai-authtest`.

### Setup Docker Hub Token (Required)

To enable automatic Docker Hub pushes, you need to set up a Docker Hub access token in your GitHub repository:

1. **Generate Docker Hub Access Token:**
   - Go to [Docker Hub](https://hub.docker.com/)
   - Log in to your account (username: `pahangkrisdyan`)
   - Click on your username → Account Settings
   - Go to "Security" → "Access Tokens"
   - Click "New Access Token"
   - Give it a name (e.g., "GitHub Actions - pahang-ai-authtest")
   - Copy the generated token (you won't be able to see it again!)

2. **Add Token to GitHub Repository:**
   - Go to your GitHub repository: https://github.com/pahangkrisdyan/pahang-ai-authtest
   - Go to Settings → Secrets and variables → Actions
   - Click "New repository secret"
   - Name: `DOCKERHUB_TOKEN`
   - Value: Paste your Docker Hub access token
   - Click "Add secret"

### Automatic Builds

Once the token is configured, the GitHub Actions workflow will:
- Build and push on every push to `main` branch
- Tag images with:
  - `latest` (for main branch)
  - `main` (branch name)
  - `main-<short-sha>` (branch + commit SHA)
  - PR number for pull requests

### Manual Docker Build

To build and run locally:

```bash
# Build the image
docker build -t pahangkrisdyan/pahang-ai-authtest .

# Run the container
docker run -d -p 8080:80 --name authtest pahangkrisdyan/pahang-ai-authtest

# Push to Docker Hub (requires docker login)
docker push pahangkrisdyan/pahang-ai-authtest
```

### Pull from Docker Hub

Once pushed, anyone can pull and run your image:

```bash
docker pull pahangkrisdyan/pahang-ai-authtest
docker run -d -p 8080:80 pahangkrisdyan/pahang-ai-authtest
```

## Project Structure

- `html/` - Static files served by nginx
  - `index.html` - Keycloak OIDC authentication example
- `Dockerfile` - nginx Alpine-based container configuration
- `.github/workflows/` - GitHub Actions CI/CD pipeline

## Configuration

The application connects to:
- Keycloak server: https://iam.pahang.ai/
- Realm: pahang.ai
- Client ID: pahang.ai

Note: The redirect URI is currently set to `https://localhost:5500/examples/javascript-vanilla/` and may need adjustment based on your deployment.
