# cloudeng-ecmp-secret

## Example values for EKS clusters sourcing from AWS Secrets Manager

```yaml
platform: "awsSecretsManager"
serviceAccountName: "crowdstrike-irsa"
region: "us-east-1"
roleArn: "arn:aws:iam::123456789012:role/ecmp-secrets-crowdstrike"

secrets:
  - name: "crowdstrike-registry-secret"
    type: "kubernetes.io/dockerconfigjson"
    secretData:
      - secretKey: ".dockerconfigjson"
        remoteKey: "CROWDSTRIKE-DOCKER-REGISTRY-SECRET"

  - name: "crowdstrike-kubeagent-registry-secret"
    type: "kubernetes.io/dockerconfigjson"
    secretData:
      - secretKey: ".dockerconfigjson"
        remoteKey: "CROWDSTRIKE-KUBEAGENT-REGISTRY-SECRET"

  - name: "crowdstrike-secret"
    type: "Opaque"
    secretData:
      - secretKey: "AGENT_CLIENT_ID"
        remoteKey: "CROWDSTRIKE-CLIENT-ID"
      - secretKey: "AGENT_CLIENT_SECRET"
        remoteKey: "CROWDSTRIKE-CLIENT-SECRET"
```

## Example values for AKS clusters sourcing from Azure Key Vault

```yaml
platform: "azureKeyVault"
serviceAccountName: "crowdstrike-wisa"
vaultUrl: "https://cloudeng-ecmp-kv.vault.azure.net"
tenantId: "0159e9d0-09a0-4edf-96ba-a3deea363c28"
msiClientId: "b98ccb08-939f-490f-a813-357a7c4dd679"

secrets:
  - name: "crowdstrike-registry-secret"
    type: "kubernetes.io/dockerconfigjson"
    secretData:
      - secretKey: ".dockerconfigjson"
        remoteKey: "CROWDSTRIKE-DOCKER-REGISTRY-SECRET"

  - name: "crowdstrike-kubeagent-registry-secret"
    type: "kubernetes.io/dockerconfigjson"
    secretData:
      - secretKey: ".dockerconfigjson"
        remoteKey: "CROWDSTRIKE-KUBEAGENT-REGISTRY-SECRET"

  - name: "crowdstrike-secret"
    type: "Opaque"
    secretData:
      - secretKey: "AGENT_CLIENT_ID"
        remoteKey: "CROWDSTRIKE-CLIENT-ID"
      - secretKey: "AGENT_CLIENT_SECRET"
        remoteKey: "CROWDSTRIKE-CLIENT-SECRET"
```

## Parameters

| Parameter | Description
| --------- | -----------
| `platform` | Platform where the secret will be used. Valid values are `awsSecretsManager` and `azureKeyVault`.
| `serviceAccountName` | The name of the service account to create which will be used to access the secret.
| `region` | The AWS region where the AWS Secrets Manager secret is located. This is required if `platform` is `awsSecretsManager`.
| `roleArn` | The ARN of the AWS role the service account should assume to access the AWS Secrets Manager secret. This is required if `platform` is `awsSecretsManager`.
| `vaultUrl` | The URL of the Azure Key Vault. This is required if `platform` is `azureKeyVault`.
| `tenantId` | The Microsoft Entra ID Tenant ID where the User Assigned MSI exists. This is required if `platform` is `azureKeyVault`.
| `identityId` | The Azure User Assigned MSI Client ID to use when authenticating to the Azure Key Vault. This is required if `platform` is `azureKeyVault`.
| `secrets[]` | The array of secrets to be created.
| `secrets.name` | Name of the secret.
| `secrets.type` | Type of the secret. This must be a valid Kubernetes secret type. Valid vales can be found [here](https://kubernetes.io/docs/concepts/configuration/secret/#secret-types).
| `secrets.secretData[]` | The array of secret data to create under this secret.
| `secrets.secretData.secretKey` | Key of the secret data.
| `secrets.secretData.remoteKey` | The remote key of the secret data. This should be the name of the secret in AWS Secrets Manager or Azure Key Vault.
