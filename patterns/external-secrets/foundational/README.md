# Foundational External Secrets Patterns

## Overview

This directory provides foundational patterns for managing secrets in Kubernetes, with a focus on utilizing external secret management systems such as AWS Secrets Manager and Azure Key Vault. These patterns are designed to help you securely inject secrets into your Kubernetes workloads while leveraging the robust capabilities of cloud providers' secrets management services.

## Directory Structure

- `aws-secrets-manager-irsa/`: Contains resources and examples for integrating AWS Secrets Manager with Kubernetes using the IAM Roles for Service Accounts (IRSA) feature.
- `aws-secrets-manager-keys/`: Provides configurations for using AWS Secrets Manager with Kubernetes, possibly using AWS access keys for authentication.
- `azure-key-vault-service-principal/`: Includes examples and guidance on setting up Azure Key Vault integration using Azure Service Principal for authentication.
- `kubernetes/`: Features syncing configuration maps and secrets from administrative clusters.

## Getting Started

To get started with these patterns:

1. Choose the directory that corresponds to the secret management system you intend to use.
2. Follow the `README.md` instructions within the selected directory for detailed setup and usage guidelines.
3. Apply the examples and configurations to your Kubernetes environment, making sure to adjust any parameters to fit your specific use case.

## Prerequisites

Before you begin, ensure you have:

- Access to a Kubernetes cluster with the necessary permissions to create and manage secrets.
- An account and permissions for the respective AWS or Azure cloud environment if you're using their secret management services.
- Familiarity with Kubernetes secret management concepts and kubectl command-line tool usage.
