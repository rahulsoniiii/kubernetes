## Secure Horizontal Pod Scaling on Kubernetes with SSL Certificates

### Introduction

Secure horizontal pod scaling is a crucial aspect of deploying applications on Kubernetes. It ensures that your application can handle increasing traffic while maintaining secure communication. In this guide, we will explore how to achieve secure horizontal pod scaling using SSL certificates from AWS Certificate Manager (ACM) and custom SSL certificates.

### Prerequisites

Before proceeding, make sure you have the following:

1. A Kubernetes cluster set up and configured.
2. The `kubectl` command-line tool installed and configured to communicate with your cluster.
3. An AWS account with access to ACM to obtain SSL certificates.

### Step 1: Obtain SSL Certificates from ACM

To secure your application, we will start by obtaining an SSL certificate from AWS Certificate Manager (ACM). ACM provides a simple way to manage and provision SSL/TLS certificates.

1. Log in to your AWS Management Console.
2. Open the ACM service.
3. Request a new certificate for your domain.
4. Choose the validation method that suits your needs (e.g., DNS validation).
5. Once the certificate is issued, make a note of its ARN (Amazon Resource Name).

### Step 2: Create a Kubernetes Secret

Next, we need to create a Kubernetes Secret that will hold the SSL certificate and private key. This Secret will be mounted into the pods for secure communication.

#### Declarative Approach:

Create a file named `ssl-secret.yaml` with the following content:

```
apiVersion: v1
kind: Secret
metadata:
  name: ssl-secret
type: kubernetes.io/tls
data:
  tls.crt: <BASE64_ENCODED_CERTIFICATE>
  tls.key: <BASE64_ENCODED_PRIVATE_KEY>
```

Replace `<BASE64_ENCODED_CERTIFICATE>` and `<BASE64_ENCODED_PRIVATE_KEY>` with the base64-encoded values of your SSL certificate and private key, respectively.

Apply the Secret to your Kubernetes cluster:

```
kubectl apply -f ssl-secret.yaml
```

#### Imperative Approach:

To create the Secret imperatively, run the following command:

```
kubectl create secret tls ssl-secret --cert=<PATH_TO_CERTIFICATE_FILE> --key=<PATH_TO_PRIVATE_KEY_FILE>
```

Replace `<PATH_TO_CERTIFICATE_FILE>` and `<PATH_TO_PRIVATE_KEY_FILE>` with the paths to your SSL certificate and private key files, respectively.

### Step 3: Enable Horizontal Pod Autoscaling

Now that we have secured the communication, let's enable horizontal pod autoscaling to handle increased traffic automatically.

1. Verify that your Kubernetes cluster has the metrics server deployed. The metrics server is responsible for collecting resource utilization metrics required for autoscaling. If it's not already deployed, you can install it using the following command:

   ```
   kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
   ```

2. Enable autoscaling for your desired deployment

### Imperative Way

   ```
   kubectl autoscale deployment my-deployment --cpu-percent=70 --min=1 --max=10
   ```

### Declarative Way

 ```
apiVersion: autoscaling/v2beta2
kind: HorizontalPodAutoscaler
metadata:
  name: my-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-deployment
  minReplicas: 1
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 70

```

   Replace `my-deployment` with the name of your deployment or StatefulSet.

   Adjust the `--cpu-percent` value as per your application's requirements and resource usage pattern. This value specifies the average CPU utilization target at which the autoscaler will adjust the number of pods.

### Conclusion

In this guide, we have learned how to achieve secure horizontal pod scaling on Kubernetes using SSL certificates from AWS