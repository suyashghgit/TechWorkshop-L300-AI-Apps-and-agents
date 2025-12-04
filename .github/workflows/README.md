# GitHub Actions Workflow Setup for Azure Container Registry

## Overview
This workflow automatically builds and deploys the Docker container to Azure Container Registry when code is pushed to the `main` branch.

## Required GitHub Secrets

You need to configure the following secrets in your GitHub repository:

1. **ACR_LOGIN_SERVER**: Your Azure Container Registry login server URL
   - Format: `<registry-name>.azurecr.io`
   - Example: `myregistry.azurecr.io`

2. **ACR_USERNAME**: Azure Container Registry username
   - This is typically the registry name itself
   - You can find this in Azure Portal under your ACR → Access keys

3. **ACR_PASSWORD**: Azure Container Registry password
   - Found in Azure Portal under your ACR → Access keys
   - Use either password1 or password2

4. **ENV**: Complete contents of your .env file
   - Copy the entire contents of your local .env file
   - This will be written to the container during build
   - Example format:
     ```
     KEY1=value1
     KEY2=value2
     KEY3=value3
     ```

## How to Add Secrets to GitHub

1. Go to your GitHub repository
2. Click on **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret**
4. Add each secret with its name and value
5. Click **Add secret**

## Workflow Details

- **Trigger**: Push to `main` branch
- **Working Directory**: `./src`
- **Image Tags**: 
  - `latest` (always points to most recent build)
  - Git commit SHA (for version tracking)

## Security Notes

- ✅ The `.env` file is already in `.gitignore` - never commit it
- ✅ The workflow creates `.env` temporarily during build and cleans it up
- ✅ All sensitive values are stored as GitHub secrets
- ✅ The Docker build context is limited to the `src/` folder

## Testing Locally

To test the Docker build locally with your .env file:

```powershell
cd src
docker build -t chat-app:local .
docker run -p 8000:8000 chat-app:local
```

## Troubleshooting

If the workflow fails:

1. **Authentication Error**: Verify ACR credentials are correct
2. **Registry Not Found**: Check ACR_LOGIN_SERVER format
3. **Build Fails**: Check Dockerfile and ensure all dependencies are in requirements.txt
4. **ENV Secret Issues**: Ensure ENV secret contains complete .env file contents
