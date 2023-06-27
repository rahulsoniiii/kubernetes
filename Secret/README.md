## Secrets in Kubernetes

Secrets in Kubernetes are used to store sensitive information such as passwords, access tokens, or SSL certificates. They can be created and managed using imperative or declarative approaches. This document provides practical examples and necessary commands for creating Secrets in Kubernetes.

### Imperative Way

#### Create a Secret
To create a Secret using the imperative approach, use the `kubectl create secret` command with the `generic` type:

```
kubectl create secret generic my-secret --from-literal=username=admin --from-literal=password=secretpassword
```

This command creates a Secret named `my-secret` with two key-value pairs: `username=admin` and `password=secretpassword`.

#### Add SSL Certificate to a Secret
To add an SSL certificate to a Secret, you can use the `kubectl create secret` command with the `tls` type:

```
kubectl create secret tls my-secret --cert=path/to/certificate.crt --key=path/to/privatekey.key
```

This command creates a Secret named `my-secret` with the SSL certificate and private key provided. Replace `path/to/certificate.crt` and `path/to/privatekey.key` with the actual paths to your certificate and private key files.

### Declarative Way

#### Create a Secret Manifest
To create a Secret using the declarative approach, create a YAML file (`secret.yaml`) with the following content:

```
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
type: Opaque
data:
  username: YWRtaW4=
  password: c2VjcmV0cGFzc3dvcmQ=
```

### Base64 Encryption and Decryption
To encode a value in Base64, you can use the echo command with the -n flag to prevent appending a newline character:

## Encryption
```
echo -n "secretpassword" | base64
```

## Decryption
```
echo -n "c2VjcmV0cGFzc3dvcmQ=" | base64 -d
```
### SSL Encrytion
```
cat ssl_certificate.crt | base64
```

Replace "ssl_certificate.crt" with the path to your SSL certificate file. The command will output the Base64-encoded representation of the certificate.

### SSL Decryption
```
echo -n "base64-encoded-certificate" | base64 --decode > ssl_certificate.crt
```

Replace "base64-encoded-certificate" with the actual Base64-encoded SSL certificate. The command will decode the certificate and save it to the file "ssl_certificate.crt".

Please note that the SSL certificate should be in PEM format for proper encoding and decoding.
In this example, the values for `username` and `password` are Base64-encoded. Replace the encoded values with your actual sensitive data.

#### Add SSL Certificate to a Secret Manifest
To add an SSL certificate to a Secret, update the YAML file (`secret.yaml`) with the following content:

```
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
type: kubernetes.io/tls
data:
  tls.crt: <Base64-encoded certificate>
  tls.key: <Base64-encoded private key>
```

Replace `<Base64-encoded certificate>` and `<Base64-encoded private key>` with the actual Base64-encoded content of your certificate and private key files.

#### Apply the Secret
To apply the Secret manifest and create the Secret, use the `kubectl apply` command:

```
kubectl apply -f secret.yaml
```

This command applies the configuration in the `secret.yaml` file and creates the Secret named `my-secret`.

### View Secrets
To view the created Secrets, use the `kubectl get secrets` command:

```
kubectl get secrets
```

This command lists all the Secrets in the current namespace.

### View Secret Details
To view the details of a specific Secret, use the `kubectl describe secret` command:

```
kubectl describe secret my-secret
```

This command displays the metadata and data of the `my-secret` Secret.

#### Accessing Secret

### Using Volume
```
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: nginx:latest
      volumeMounts:
        - name: secret-volume
          mountPath: /path/to/mount/secret
  volumes:
    - name: secret-volume
      secret:
        secretName: my-secret
```


### Using Environment Variable
```
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: nginx:latest
      env:
        - name: SECRET_USERNAME
          valueFrom:
            secretKeyRef:
              name: my-secret
              key: username
        - name: SECRET_PASSWORD
          valueFrom:
            secretKeyRef:
              name: my-secret
              key: password
```

## Conclusion
Creating Secrets in Kubernetes can be done using either the imperative or declarative approach. The imperative way involves using commands like `kubectl create secret`, while the declarative way uses YAML manifests and the `kubectl apply` command. Choose the approach that best suits your needs and preferences.

Remember to handle Secrets with care, as they contain sensitive information. Ensure proper access controls and encryption mechanisms are in place to protect Secrets within your Kubernetes cluster.