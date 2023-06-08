# Kubernetes Pods - Quick Reference Guide

This guide provides a quick reference for commonly used commands when working with Kubernetes Pods.

## Creating and Managing Pods

- Apply a YAML configuration file:
  ```
  kubectl apply -f <script.yml>
  ```

- Get a list of pods:
  ```
  kubectl get pods
  ```

- Get detailed information about pods:
  ```
  kubectl describe pod <pod_name>
  ```

- Delete a pod:
  ```
  kubectl delete pod <pod_name>
  ```

- Delete pods defined in a YAML configuration file:
  ```
  kubectl delete -f <script.yml>
  ```

## Viewing Pod Information

- Get pods and display additional information like IP and node:
  ```
  kubectl get pods -o wide
  ```

- Get pods and output the details in YAML format:
  ```
  kubectl get pods -o yaml
  ```

- Get pods and output the details in JSON format:
  ```
  kubectl get pods -o json
  ```

- View the logs of a pod:
  ```
  kubectl logs <pod_name>
  ```

- Stream the logs of a pod:
  ```
  kubectl logs -f <pod_name>
  ```

- Stream the logs of a specific container within a pod:
  ```
  kubectl logs -f <pod_name> -c <container_name>
  ```

- Access the shell of a container within a pod:
  ```
  kubectl exec <pod_name> -it -c <container_name> -- /bin/bash
  ```

## Working with Labels

- Show labels for pods:
  ```
  kubectl get pods --show-labels
  ```

- Add a label to a pod:
  ```
  kubectl label pods <pod_name> <key>=<value>
  ```

- Get pods with a specific label:
  ```
  kubectl get pods -l <key>=<value>
  ```

- Get pods without a specific label:
  ```
  kubectl get pods -l <key>!=<value>
  ```

- Get pods with a label matching multiple values:
  ```
  kubectl get pods -l '<key> in (<value1>,<value2>)'
  ```

- Get pods with a label not matching multiple values:
  ```
  kubectl get pods -l '<key> notin (<value1>,<value2>)'
  ```

## Working with Nodes

- Get a list of nodes:
  ```
  kubectl get nodes
  ```

- Add a label to a node:
  ```
  kubectl label nodes <node_name> <key>=<value>
  ```

Feel free to explore these commands and experiment with Kubernetes Pods. Happy container orchestration!

#Kubernetes #Pods #ContainerOrchestration