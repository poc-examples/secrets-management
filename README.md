# Managing Secrets in OpenShift

- Slide Deck: [Demonstration](https://docs.google.com/presentation/d/17zfCQTKKynqM6dPdprr5_7CHdMGJiaZQ_3Ft_Gqog5Q/view)
- Demonstration: [Teams Based External Secrets](https://www.youtube.com/)

In Kubernetes environments, secrets management is crucial for safeguarding sensitive data such as passwords, tokens, and keys, ensuring that this information is securely stored, transmitted, and accessed only by authorized entities. 

Without proper secrets management, sensitive data could inadvertently be exposed, leading to security vulnerabilities and potential breaches. Furthermore, it is imperative to keep secrets out of version control systems like Git, as these platforms are not designed for storing sensitive information. Including secrets in Git repositories, even in private ones, can lead to accidental exposure if the repository is cloned, shared, or becomes public, significantly increasing the risk of data leakage and unauthorized access. 

Effective secrets management in Kubernetes helps in maintaining robust security practices, compliance with data protection regulations, and the overall integrity of the application environment.

## Available Patterns

### External-Secrets

- **Foundational Patterns** - [Example](https://github.com/poc-examples/managing-secrets/tree/main/patterns/external-secrets/foundational) - Bootstrapping the operator, authenticating a ClusterSecretStore, and pulling a secret.
- **Team Based** - [Example](https://github.com/poc-examples/managing-secrets/tree/main/patterns/external-secrets/teams-based) - A Pattern for allowing teams to control their own External Secrets.

### Secrets-CSI-Driver
- TBC

### Hybrid Configuration
- TBC

## Why These Patterns

The secret-csi-driver is primarily focused on securely injecting secrets into pods as volumes. This mechanism is particularly useful for applications that need to consume secrets in a way that is both secure and transparent to the application code, reducing the complexity of secret management within the application logic itself.

On the other hand, the external-secrets-operator facilitates the integration of OpenShift with external secrets management systems. This is crucial for environments where secrets need to be managed centrally outside of the OpenShift cluster, allowing for consistent secret management practices across various platforms and reducing the overhead of managing secrets directly within OpenShift.

The decision to use one or both of these tools would depend on the specific requirements of your secrets management strategy. If your environment requires robust integration with external secret stores and secure, efficient injection of those secrets into OpenShift workloads, leveraging both tools together could offer a comprehensive solution

## How It Works

1. **Creation and Storage**: Secrets are created and stored within the Kubernetes etcd database, a key-value store used to hold all cluster data. These secrets are encoded in base64 format, which is not encryption but merely an encoding mechanism to handle binary data. It is crucial that access to etcd is well-protected and that etcd itself is configured to store data securely, potentially using additional encryption methods.

2. **Access Control**: Access to secrets is strictly regulated through Kubernetes Role-Based Access Control (RBAC) policies. These policies define who (which users or applications) can access or modify the secrets. By implementing strict access controls, you can ensure that only authorized entities can retrieve or manipulate the secret data.

3. **Usage**: When a pod needs to access a secret, the secret is mounted as a volume or exposed as an environment variable within the container, based on the specifications in the pod definition. This approach keeps secrets abstracted away from application code, reducing the risk of accidental exposure.

4. **Rotation and Management**: Secrets should be periodically rotated to minimize the risks associated with potential leaks or unauthorized access. Kubernetes allows for updating secrets without redeploying the associated pods. Once a secret is updated, the mounted volumes within the pods are automatically updated as well.

5. **Avoiding Git Storage**: To prevent secrets from being stored in Git, it is essential to exclude secret objects from source code and configuration files. Instead of hardcoding secrets, applications should be designed to retrieve them dynamically from Kubernetes at runtime. Additionally, tools like Git hooks and linters can help prevent accidental commits of sensitive data.

## Prerequisites

> **Blank OpenShift Cluster** - Have a fresh OpenShift cluster installed before attempting to create these reference patterns.

## Getting Started

**Bootstrap the Cluster**: Reference the repository [poc-examples/bootstrap-openshift](https://github.com/poc-examples/bootstrap-openshift), configure the demo settings.


```
openshift:

  token: "<OPENSHIFT_TOKEN>"
  api_url: "<OPENSHIFT_API_URL>"
  validate_certs: false

  # machines:

  #   - spot: true
  #     gpu: true
  #     replicas: 1
  #     location: eastus
  #     zone: "1"

operators:

  gitops:
    namespace: openshift-operators
    channel: gitops-1.11

  external_secrets:
    namespace: openshift-operators
    channel: stable

secrets:

  namespace: openshift-operators

  # azure:
  #   client_id: "<CLIENT_ID>"
  #   client_secret: "<CLIENT_SECRET>"
  #   tenant_id: "<TENANT_ID>"
  #   vault_url: "<VAULT_URL>"

  # aws:
  #   access_key_id: "<AWS_ACCESS_KEY_ID>"
  #   secret_access_key: "<AWS_SECRET_ACCESS_KEY>"
  #   default_region: "<AWS_DEFAULT_REGION>"

gitops:

  namespace: openshift-gitops

  plugins:
    - repo: poc-examples/managing-secrets
      path: patterns/aws-keys
      name: managing-secrets-demo

external_secrets:
  namespace: external-secrets
```

Execute the bootstrapper.
```
make init
```

## Running the Tests

Explain how to run the automated tests for this system. Include any relevant commands.

## Tools Used

- OpenShift
- Ansible
- External Secrets Operator
- Secrets CSI Driver
- Argo CD

## Contributing

Please read [CONTRIBUTING.md](https://github.com/poc-examples/demo-repo-base/blob/main/CONTRIBUTING.md) for details on our code of conduct, and the process for submitting pull requests to us.

## Versioning

We use SemVer for versioning. For the versions available, see the tags on this repository.

## Authors

- Christopher Engleby - [Linked-In](https://www.linkedin.com/in/christopher-engleby-116a87aa/)

See also the list of contributors who participated in this project.

## License

This project is licensed under the LICENSE.md file for details.

## Acknowledgments

Hat tip to anyone whose code was used Inspiration.
