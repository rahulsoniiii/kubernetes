Step 1: Obtain the SSL Certificate and Key
------------------------------------------
1. Purchase and obtain an SSL certificate from Name.com. They will typically provide you with the certificate file (e.g., `certificate.crt`) and the private key file (e.g., `private.key`).

Step 2: Create a Kubernetes Secret
---------------------------------
1. Create a TLS secret in Kubernetes using the certificate and key files obtained from Name.com. Run the following command:

   ```
   kubectl create secret tls my-certificate --cert=path/to/certificate.crt --key=path/to/private.key --namespace=my-namespace
   ```

   Replace `my-certificate` with the desired name for the secret, `path/to/certificate.crt` with the path to your certificate file, `path/to/private.key` with the path to your private key file, and `my-namespace` with the namespace where you want to create the secret.

Step 3: Deploy the AWS ALB Ingress Controller as a Kubernetes deployment
------------------------------------------------------------------------
   ```
   kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/aws-alb-ingress-controller/v1.1.8/docs/examples/alb-ingress-controller.yaml
   ```

Step 4: Configure the NGINX Ingress
----------------------------------
1. Create an Ingress resource manifest (e.g., `my-ingress.yaml`) with the following content:

   ```
   apiVersion: networking.k8s.io/v1
   kind: Ingress
   metadata:
     name: my-ingress
     annotations:
       nginx.ingress.kubernetes.io/ssl-redirect: "true"
       nginx.ingress.kubernetes.io/ssl-passthrough: "true"
   spec:
     tls:
       - hosts:
           - your-domain.com
         secretName: my-certificate
     rules:
       - host: your-domain.com
         http:
           paths:
             - path: /
               pathType: Prefix
               backend:
                 service:
                   name: your-service
                   port:
                     number: 80
   ```

   Replace `my-ingress` with the desired name for the Ingress resource, `your-domain.com` with your actual domain name, `my-certificate` with the name of the secret created in Step 2, and `your-service` with the name of your backend service.

2. Apply the Ingress resource:

   ```
   kubectl apply -f my-ingress.yaml
   ```

   This will create the Ingress resource and configure NGINX Ingress to use the specified certificate for TLS termination.

That's it! With these steps, you should have NGINX Ingress configured to use your Name.com SSL certificate for secure HTTPS traffic.