# External Secrets - Teams Based Secrets Management

The pattern depicted in the image illustrates an approach to managing secrets in an OpenShift environment using the External Secrets Operator and Azure Key Vault. The process is divided into two main roles: Admin and Team, each with specific steps to securely manage and utilize secrets.

![Teams Based Secrets](https://raw.githubusercontent.com/poc-examples/managing-secrets/main/images/team-based-secrets.png "Teams Based Secrets")

## Admin role:

1. The External Secrets Operator is deployed into the `openshift-operators` namespace.
2. A service principal (which is an Azure identity) is created in the `openshift-operators` namespace. This service principal has credentials that are used to interact with Azure Key Vault.
3. In an admin-controlled namespace, the admin creates ClusterSecretStore resources with limited namespace access. These are linked to the Azure Key Vault using the service principal credentials. The ClusterSecretStore is an OpenShift resource that references an external store where the actual secrets are kept.

## Team role:

1. Each team can create ExternalSecrets in their own namespaces by referencing the ClusterSecretStore. These ExternalSecrets are custom resources that define which secrets should be pulled from Azure Key Vault.
2. The secrets are then synced to the teams' namespaces and can be used in deployments. This synchronization allows for the separation of duties, as the team does not need direct access to Azure Key Vault; they only interact with the ExternalSecrets.
3. Since the ClusterSecretStore is not scoped to the teams' namespaces, team members cannot directly access the Azure Key Vault or the service principal credentials, adding a layer of security.

This pattern ensures that sensitive credentials are not directly handled by the teams and provides a centralized way of managing access to secrets. It allows for fine-grained control over secrets and their distribution across different teams and namespaces within the OpenShift cluster.
