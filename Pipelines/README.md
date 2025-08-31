# CI/CD Pipeline for Rupesh Profile Website

This Azure DevOps pipeline builds and deploys the Rupesh Profile website Docker image to Azure Container Registry (ACR).

## Pipeline Overview

The pipeline consists of four main stages:

1. **Build Stage**: Builds the Docker image and runs tests
2. **Push to ACR Stage**: Pushes the image to Azure Container Registry
3. **Deploy Stage**: Deploys the application to production (main branch only)
4. **Notification Stage**: Sends build notifications

## Prerequisites

### Azure Resources Required

1. **Azure Container Registry (ACR)**
   - Create an ACR instance in your Azure subscription
   - Note the ACR name for pipeline variables

2. **Azure Service Connection**
   - Create a service connection in Azure DevOps
   - Grant necessary permissions to access ACR

### Pipeline Variables

Set the following variables in your Azure DevOps pipeline:

| Variable Name | Description | Example |
|---------------|-------------|---------|
| `ACR_NAME` | Your Azure Container Registry name | `myacr123` |
| `AZURE_SUBSCRIPTION` | Azure service connection name | `MyAzureSubscription` |
| `RESOURCE_GROUP` | Azure resource group for deployment | `my-resource-group` |
| `ACR_USERNAME` | ACR username (optional) | `myacr123` |
| `ACR_PASSWORD` | ACR password (optional) | `secret-password` |

## Pipeline Features

### 🔒 Security
- Security scanning with Trivy
- Non-root user in Docker container
- Security headers in nginx configuration

### 🧪 Testing
- Container integration tests
- Health check validation
- Application response verification

### 🏷️ Image Tagging
- Build ID tag for versioning
- Latest tag for easy access
- Environment-specific tags (dev, production)

### 📊 Monitoring
- Build status notifications
- Health check endpoints
- Comprehensive logging

## Usage

### Triggering the Pipeline

The pipeline triggers on:
- **Branches**: `main`, `dev`, `feature/*`
- **Paths**: Changes in `RupeshProfile/*` directory
- **Pull Requests**: To `main` and `dev` branches

### Manual Execution

You can manually trigger the pipeline from:
1. Azure DevOps → Pipelines → Your Pipeline
2. Click "Run pipeline"
3. Select branch and parameters

## Deployment Options

The pipeline includes example deployment configurations for:

### Azure Container Instances (ACI)
```bash
az container create \
  --resource-group $(RESOURCE_GROUP) \
  --name rupesh-profile-aci \
  --image $(acrLoginServer)/$(imageName):$(imageTag) \
  --dns-name-label rupesh-profile \
  --ports 80 \
  --registry-login-server $(acrLoginServer) \
  --registry-username $(ACR_USERNAME) \
  --registry-password $(ACR_PASSWORD)
```

### Azure Kubernetes Service (AKS)
- Use the Kubernetes manifests in `RupeshProfile/manifest/`
- Update image references in deployment files

### Azure App Service
- Configure container deployment
- Set image reference to ACR

### Azure Container Apps
- Use Azure Container Apps for serverless deployment

## Customization

### Adding New Environments

1. Add new environment variables
2. Create new deployment stages
3. Update conditions for environment-specific deployments

### Notification Integration

The pipeline includes a notification stage that can be extended to:
- Send Teams notifications
- Send Slack messages
- Send email notifications
- Update status in external systems

### Security Enhancements

Consider adding:
- Vulnerability scanning with Azure Security Center
- Image signing with Notary
- Policy enforcement with Azure Policy

## Troubleshooting

### Common Issues

1. **ACR Login Failed**
   - Verify service connection permissions
   - Check ACR name variable

2. **Docker Build Failed**
   - Verify Dockerfile path
   - Check build context

3. **Container Tests Failed**
   - Verify application starts correctly
   - Check health endpoint configuration

### Debug Mode

Enable debug logging by setting:
```yaml
variables:
  system.debug: true
```

## Best Practices

1. **Branch Protection**: Enable branch protection on main branch
2. **Approvals**: Require approvals for production deployments
3. **Testing**: Add comprehensive tests before deployment
4. **Monitoring**: Set up application monitoring and alerting
5. **Backup**: Implement backup strategies for production data

## Support

For issues or questions:
1. Check Azure DevOps pipeline logs
2. Review Azure Container Registry logs
3. Verify Azure service connection permissions
4. Contact your Azure DevOps administrator
