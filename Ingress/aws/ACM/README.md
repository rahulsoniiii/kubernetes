AWS Ingress Setup with SSL/TLS Termination on AWS Kops
=====================================================

Prerequisites:
--------------
- An AWS Kubernetes cluster created with Kops.
- `kubectl` and `awscli` installed and configured.

Step 1: Install the AWS ALB Ingress Controller:
----------------------------------------------
1. Deploy the AWS NLB Ingress Controller as a Kubernetes deployment:
   ```
   kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.3/deploy/static/provider/aws/deploy.yaml
   ```

2. Verify that the controller is running:
   ```
   kubectl get pods -n kube-system
   ```

Step 2: Request an SSL/TLS Certificate:
--------------------------------------
1. Open the AWS Certificate Manager (ACM) console.
2. Click "Request a certificate" and enter your domain name(s).
3. Choose a validation method and complete the validation process.
4. Once the certificate is issued, make note of the certificate ARN.

Step 3: Create an Ingress Resource:
----------------------------------
1. Create an Ingress resource manifest file (e.g., `ingress.yaml`) with the following content:
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
  annotations:
    kubernetes.io/ingress.class: alb
    alb.ingress.kubernetes.io/certificate-arn: <certificate-arn>
spec:
  rules:
    - host: my-domain.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: my-service
                port:
                  number: 80
```
  **Replace `<certificate-arn>` with the ARN of your SSL/TLS certificate.**

2. Apply the Ingress resource:
   ```
   kubectl apply -f ingress.yaml
   ```

Step 4: Verify the Setup:
-------------------------
1. Get the ELB hostname:
   ```
   kubectl get ingress my-ingress -o jsonpath='{.status.loadBalancer.ingress[0].hostname}'
   ```

2. Add a DNS record pointing to the ALB hostname using your DNS provider.

3. Access your service using HTTPS at the defined domain.