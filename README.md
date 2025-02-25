# Vault-CRD

This helm chart installs Vault-CRD version v1.11.0
https://vault.koudingspawn.de

## Prerequisites

### Configure Vault for Usage with Vault-CRD
Please follow the instructions on how to install Vault-CRD. You can specify which of the supported Authentication methods should be used:
https://vault.koudingspawn.de/install-vault-crd

### Configuration
Please change the values.yaml according to your setup
See here for the official documentation https://vault.koudingspawn.de/install-vault-crd#configuration-of-vault-crd

Parameter | Description | Default
--- | --- | ---
`vaultCRD.repository` | Image repository | `daspawnw/vault-crd`
`vaultCRD.tag` | Image tag | `1.11.0`
`vaultCRD.pullPolicy` | Image pull policy | `IfNotPresent`
`vaultCRD.vaultUrl` | Please specify here the URL to your Vault installation. Don't forget to set the /v1/ path e.g. http://localhost:8080/v1/ | `nil`
`vaultCRD.vaultAuth` | Specifies the used authentication method the following values are allowed: token / serviceAccount | `nil`
`vaultCRD.vaultToken` | Token with access to the resources that Vault-CRD shares from Vault to Kubernetes. Required if vaultAuth = token | `nil`
`vaultCRD.vaultRole` | If you use the Service Account approach for Vault authentication please specify here the Vault role. Required if vaultAuth = serviceAccount | `nil`
`vaultCRD.rbac` | Should it generate rbac resources | `true`
`vaultCRD.serviceAccountName` | Name of a service account that should be created | `vault-crd-serviceaccount`
`vaultCRD.memory` | JVM Max memory in mb | `256`
`vaultCRD.memoryLimit` | Container max memory in mb should be 20% higher then jvm memory value | `307`
`vaultCRD.admissionWebhook.enabled` | Enable or disable admission webhook to verify if secrets are accessible before apply | `false`
`vaultCRD.admissionWebhook.certBase64` | Certificate used for serving admission webhook request in Vault-CRD, can be self signed | ""
`vaultCRD.admissionWebhook.keyBase64` | Key used for serving admission webhoook request in Vault-CRD, can be self signed | ""
`vaultCRD.admissionWebhook.caBase64` | Root certificate used to allow apiserver to validate Server tls, can be certBase64 | ""


## How to

Add Helm repository

```
helm repo add vault-crd https://raw.githubusercontent.com/mikle7771/vault-crd-helm/master
```

When used with serviceAccount:

```
helm install --name vault --namespace vault-crd vault-crd/vault-crd \
    --set vaultCRD.vaultUrl=http://localhost:8080/v1/ \
    --set vaultCRD.vaultAuth=serviceAccount \
    --set vaultCRD.vaultRole=test
```

When used with Token:

```
helm install --name vault --namespace vault-crd vault-crd/vault-crd \
    --set vaultCRD.vaultUrl=http://localhost:8080/v1/ \
    --set vaultCRD.vaultToken=uuid
```
