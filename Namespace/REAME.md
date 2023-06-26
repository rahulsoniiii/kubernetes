### **Namespaces Documentation**

Namespaces in Kubernetes provide a way to create virtual clusters within a physical cluster. They help in organizing and isolating resources, enabling multiple teams or applications to coexist without interference.

#### Creating Namespaces
1. **Implicit Creation:** When you create resources without specifying a namespace, they are created in the default namespace. Implicit creation is the default behavior in Kubernetes.
2. **Declarative Creation:** You can create namespaces explicitly by defining them in YAML files and applying them using the following command:
   
   ```
   kubectl apply -f <namespace.yaml>
   ```

#### General Commands
- Create a new namespace:
  ```
  kubectl create namespace <namespace-name>
  ```

- List all namespaces:
  ```
  kubectl get namespaces
  ```

- Get detailed information about a specific namespace:
  ```
  kubectl describe namespace <namespace-name>
  ```

- Delete a namespace and all its resources:
  ```
  kubectl delete namespace <namespace-name>
  ```

- List all pods in a specific namespace:
  ```
  kubectl get pods -n <namespace-name>
  ```

- Set a namespace as the default for the current context:
  ```
  kubectl config set-context $(kubectl config current-context) --namespace=<namespace-name>
  ```

#### Manifest
You can use the following YAML manifest to create a namespace named "test":

```
apiVersion: v1
kind: Namespace
metadata:
  name: test
```

Apply the manifest using the command `kubectl apply -f namespace.yaml`.