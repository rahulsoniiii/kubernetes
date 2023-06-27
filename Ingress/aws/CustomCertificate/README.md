To use a custom SSL certificate for your AWS Ingress setup, you'll need to generate the key and certificate files and then create a Kubernetes Secret to store them. Here's a step-by-step guide along with documentation references for each step:

Step 1: Generate the Key and Certificate Files:
----------------------------------------------
1. Generate a private key file (e.g., `private.key`):
   ```
   openssl genrsa -out private.key 2048
   ```
2. Create a CSR configuration file:

    1. Create a new text file (e.g., csr.conf) and open it in a text editor.
    2. Add the following content to the file, replacing the placeholder values with your own information:

    ```
    [req]
    default_bits = 2048
    prompt = no
    default_md = sha256
    req_extensions = req_ext
    distinguished_name = dn

    [dn]
    CN = your_domain.com  # Replace with your domain name

    [req_ext]
    subjectAltName = @alt_names

    [alt_names]
    DNS.1 = your_domain.com  # Replace with your domain name
    ```

2. Generate a certificate signing request (CSR) file (e.g., `csr.pem`):
   ```
    openssl req -new -sha256 -key private.key -out csr.csr -config csr.conf
   ```

3. Submit the CSR to a certificate authority (CA) or use an online service to obtain the SSL certificate. The CA will provide you with the certificate file (e.g., `certificate.crt`) and any intermediate certificate files if applicable.

Note: The exact steps for obtaining a custom SSL certificate may vary depending on the CA or service you choose. Refer to their documentation or support resources for specific instructions.

Step 2: Create a Kubernetes Secret:
----------------------------------
1. Create a YAML file (e.g., `ssl-secret.yaml`) with the following content:
   ```
   apiVersion: v1
   kind: Secret
   metadata:
     name: ssl-secret
     namespace: your-namespace
   type: kubernetes.io/tls
   data:
     tls.crt: <base64-encoded-certificate>
     tls.key: <base64-encoded-private-key>
   ```
   Replace `<base64-encoded-certificate>` and `<base64-encoded-private-key>` with the base64-encoded content of your certificate and private key files, respectively. You can encode them using the `base64` command-line tool:
   ```
   cat certificate.crt | base64
   cat private.key | base64
   ```

   # Create the Secret from literal values
   ```
    kubectl create secret tls ssl-secret --cert='base64-encoded-certificate' --key='base64-encoded-private-key' --namespace=your-namespace
   ```
   # Create the Secret from files
    ```
    kubectl create secret tls ssl-secret --cert=path/to/certificate.crt --key=path/to/private.key --namespace=your-namespace
    ```

   # For decoding
    ```
    echo "base64-encoded-string" | base64 --decode
    ```

2. Apply the Secret:
   ```
   kubectl apply -f ssl-secret.yaml
   ```

Step 3: Deploy the AWS ALB Ingress Controller with the Secret:
--------------------------------------------------------------
1. Follow the AWS ALB Ingress Controller installation guide to deploy the controller in your cluster. Use the `--aws-secret-name` flag to specify the Secret containing the custom SSL certificate:
   ```bash
   kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/aws-alb-ingress-controller/v1.1.8/docs/examples/alb-ingress-controller.yaml --namespace kube-system --set awsSecretName=ssl-secret
   ```

2. Verify that the controller is running:
   ```
   kubectl get pods -n kube-system
   ```

Step 4: Create an Ingress Resource:
----------------------------------
1. Create an Ingress resource manifest file (e.g., `ingress.yaml`) with the following content:
   ```yaml
   apiVersion: networking.k8s.io/v1
   kind: Ingress
   metadata:
     name: my-ingress
     annotations:
       kubernetes.io/ingress.class: alb
       alb.ingress.kubernetes.io/ssl-certificate: ssl-secret
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

2. Apply the Ingress resource:
   ```
   kubectl apply -f ingress.yaml
   ```

