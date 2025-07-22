# AWS Serverless SaaS Workshop

A hands-on implementation of a multi-tenant Software-as-a-Service (SaaS) application built entirely on AWS serverless technologies. This workshop demonstrates how to architect, deploy, and manage a production-ready SaaS platform using modern serverless patterns.

## 🚀 Project Overview

This project showcases the implementation of a comprehensive SaaS platform that addresses the core challenges of multi-tenant architecture through serverless technologies. The solution demonstrates how to build scalable, cost-effective SaaS applications while maintaining strict tenant isolation and operational efficiency.

### Key Features

- **Multi-tenant Architecture**: Support for both pooled and dedicated tenant models
- **Serverless-First Design**: Built entirely on AWS serverless services for optimal cost and performance
- **Three-Tier Architecture**: Web applications, shared services, and application services layers
- **Automated Deployment**: Infrastructure as Code with AWS SAM and CDK
- **Comprehensive Monitoring**: Built-in observability with CloudWatch, X-Ray, and Lambda Insights

## 🏗️ Architecture

The solution implements a sophisticated three-tiered architecture:

### Web Applications Layer
- **SaaS Provider Admin Console**: Angular-based management interface for platform administrators
- **Landing/Sign-up Application**: Public registration portal for new tenant onboarding
- **Sample SaaS Commerce Application**: Full-featured e-commerce demo with authentication

### Shared Services Layer
- **Tenant Management Service**: Handles tenant provisioning and lifecycle management
- **User Management Service**: Manages authentication and authorization across tenants
- **Onboarding Service**: Automates the complete tenant registration process

### Application Services Layer (Tiered Deployment)
- **Pooled Model** (Basic/Standard/Premium): Shared AWS resources with logical isolation
- **Silo Model** (Platinum): Dedicated resources for enterprise customers

## 🛠️ Technology Stack

### Core AWS Services
- **AWS Lambda**: Serverless compute with automatic scaling
- **Amazon API Gateway**: REST APIs with built-in throttling and usage plans
- **Amazon DynamoDB**: Serverless NoSQL database with on-demand scaling
- **Amazon Cognito**: User authentication and authorization
- **AWS SAM/CDK**: Infrastructure as Code deployment
- **AWS CodePipeline**: CI/CD automation

### Development Stack
- **Python 3.9**: Primary programming language for Lambda functions
- **Angular**: Frontend framework for web applications
- **Node.js**: Build tools and CDK development

## 📋 Prerequisites

Before starting, ensure you have:
- AWS Account with appropriate permissions
- AWS CLI configured
- Node.js 14+ installed
- Python 3.8+ installed
- Git installed

## 🚀 Getting Started

### 1. Clone the Repository
```bash
git clone https://github.com/aws-samples/aws-serverless-saas-workshop.git
cd aws-serverless-saas-workshop
```

### 2. Environment Setup (Cloud9)
If using AWS Cloud9, increase disk space:
```bash
cd Cloud9Setup/
./increase-disk-size.sh
```

### 3. Install Prerequisites
```bash
cd Cloud9Setup/
./pre-requisites.sh
```

### 4. Verify Installation
```bash
./pre-requisites-versions-check.sh
```

Expected output:
```
***************SUMMARY****************
* PASS : Python version 3.8.11 installed
* PASS : aws-cli/2.2.43 version installed
* PASS : Has required t3.large instance type
* PASS : Has minimum required 50GiB volume size
* PASS : Sam cli version 1.64.0 installed
* PASS : Has git-remote-codecommit installed
* PASS : Node version 14.18.0 installed
* PASS : CDK version 2.40.0 installed
***************END OF SUMMARY*********
```

### 5. Configure API Gateway CloudWatch Logging
1. Navigate to API Gateway console
2. Go to Settings → CloudWatch log role ARN
3. Ensure proper IAM role is configured for logging

## 🔧 Deployment

### Deploy the Complete Stack
```bash
cd Lab1/scripts/
./deployment.sh -s -c --stack-name serverless-saas-workshop-lab1
```

Parameters:
- `-s`: Deploy server-side components (Lambda, API Gateway, DynamoDB)
- `-c`: Deploy client-side components (Angular apps, static assets)
- `--stack-name`: CloudFormation stack name

### Deployment Verification
After successful deployment, the script outputs the application URL. You can access the SaaS platform through this URL.

## 🧪 Testing the Implementation

### 1. Retrieve API Gateway Information
```bash
aws apigateway get-rest-apis --region <your-region> --query 'items[].{Name:name,Id:id}'
```

### 2. Add Test Product
```bash
curl -X POST https://<api-id>.execute-api.<region>.amazonaws.com/prod/product \
  -H 'Content-Type: application/json' \
  -d '{"category": "electronics", "name": "Smart Speaker", "price": "99", "sku": "SP001"}'
```

### 3. Verify Data Storage
```bash
aws dynamodb scan --table-name "Product-Lab1" --region <your-region>
```

## 📊 Monitoring and Observability

### CloudWatch Logs
- Navigate to CloudWatch → Log groups
- Look for function-specific log groups (e.g., CreateProductFunction)
- Monitor request patterns and error rates

### Lambda Insights
Each function includes Lambda Insights extension for enhanced monitoring:
- Performance metrics
- Memory utilization
- Cold start analysis

### X-Ray Tracing
Distributed tracing is enabled across all services:
- Request flow visualization
- Performance bottleneck identification
- Error analysis

## 💰 Cost Optimization

### Serverless Economics
- **Pay-per-use**: Only pay for actual requests and compute time
- **Automatic scaling**: No idle resource costs
- **DynamoDB on-demand**: Eliminates capacity planning overhead

### Resource Configuration
- Lambda functions sized appropriately (128MB for lightweight operations)
- API Gateway usage plans for tenant-specific billing
- CloudWatch log retention policies to manage storage costs

## 🔒 Security Implementation

### Authentication & Authorization
- **Cognito User Pools**: Secure user management with built-in security features
- **Lambda Authorizers**: Custom authorization logic for tenant-specific access
- **IAM Roles**: Least privilege access for all Lambda functions

### Data Isolation
- Application-level tenant identification
- Database-level access controls
- API-level request validation

## 🎯 Key Learning Outcomes

### SaaS Architecture Patterns
1. **Multi-tenant data isolation** in shared infrastructure
2. **Tiered deployment strategies** for different customer segments
3. **Automated tenant onboarding** workflows
4. **Cost attribution** in pooled resources

### Serverless Best Practices
1. **Infrastructure as Code** with SAM and CDK
2. **Event-driven architecture** design patterns
3. **Monitoring and observability** implementation
4. **Security-first** development approach

## 🚦 Common Issues and Solutions

### Lambda Deployment Issues
When updating Lambda functions, use proper deployment packages:
```bash
aws lambda update-function-code \
  --function-name "function-name" \
  --zip-file fileb://deployment.zip \
  --region <region>
```

### API Gateway Testing
Always test through API Gateway endpoints rather than direct Lambda invocation:
- API Gateway events have different structure than direct invocation
- Use proper HTTP headers and request formatting

### Environment Variables
Key environment variables in Lambda functions:
- `PRODUCT_TABLE_NAME`: DynamoDB table name
- `LOG_LEVEL`: Logging verbosity (DEBUG/INFO/ERROR)
- `POWERTOOLS_SERVICE_NAME`: Service identification for observability

## 🛣️ Roadmap and Future Enhancements

This implementation serves as the foundation for advanced SaaS features:

### Phase 2: Advanced Multi-Tenancy
- Tenant-aware logging and monitoring
- Dynamic tier migration based on usage
- Cross-tenant analytics with data isolation

### Phase 3: Enterprise Features
- Single sign-on integration
- Advanced compliance reporting
- Third-party service integrations

### Phase 4: Platform Optimization
- Advanced cost attribution algorithms
- Predictive scaling based on tenant patterns
- Enhanced security with tenant-specific policies

## 🤝 Contributing

This project follows the AWS Samples contribution guidelines:
1. Fork the repository
2. Create a feature branch
3. Make your changes with proper testing
4. Submit a pull request with detailed description

## 📝 License

This project is licensed under the MIT-0 License. See the LICENSE file for details.

## 🔗 Additional Resources

- [AWS Serverless Application Lens](https://docs.aws.amazon.com/wellarchitected/latest/serverless-applications-lens/)
- [SaaS Architecture Patterns](https://aws.amazon.com/partners/saas-factory/)
- [AWS Lambda Best Practices](https://docs.aws.amazon.com/lambda/latest/dg/best-practices.html)
- [Multi-Tenant SaaS on AWS](https://aws.amazon.com/solutions/implementations/saas-identity-cognito/)

## 💬 Support

For questions and support:
- AWS SaaS Factory: [aws-saas-factory@amazon.com](mailto:aws-saas-factory@amazon.com)
- AWS Documentation: [AWS Serverless Resources](https://aws.amazon.com/serverless/)
- Community Support: [AWS Developer Forums](https://forums.aws.amazon.com/)

---

**Note**: This workshop demonstrates production-ready patterns but should be thoroughly tested and customized for your specific use case before deploying to production environments.
