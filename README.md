# alist Kustomize Configuration Repository  

**[中文文档](./README_zh.md)**  

This repository provides an easy way to deploy [alist](https://alist.nn.ci) on Kubernetes using [Kustomize](https://kustomize.io/).  


## Usage  

First, clone this repository to your local machine:

```bash
git clone https://github.com/sujoshua/alist-kustomize.git
cd alist-kustomize
```

Go to the `overlay` directory, which contains configuration templatesfor different environments:

```bash
cd overlay
```

Copy the `example` directory and rename it to `prod` (or any othername such as `dev`, `staging`, depending on your environment):

```bash
cp -r example prod
cd prod
```

Modify the configuration files in the `prod` directory according toyour requirements. Refer to the [Configuration Details](#configuration-details) section below for what can be customized.

In the `prod` directory, preview the generated YAML configurationwith:

```bash
kustomize build .
```

Or using `kubectl` for a dry-run:

```bash
kubectl apply -k . --dry-run=client
```
Once confirmed the configuration is correct, apply it to yourKubernetes cluster using:
```bash
kubectl apply -k .
```
---

## Configuration Details  

### 1. kustomization.yaml  
The `kustomization.yaml` file is the main configuration file for Kustomize. It defines resources, generates ConfigMaps, sets image versions, applies patches, and more.

| Configuration Item             | Description                               | Editable Options                   |
|--------------------------------|-------------------------------------------|-------------------------------------|
| `namespace`                    | Namespace for all resources                | Can be changed to a custom namespace |
| `namePrefix`                   | Adds a prefix to all resource names         | Can be modified or removed           |
| `labels`                       | Adds common labels to all resources         | Can add or modify labels as needed   |
| `configMapGenerator.env`        | Dynamically generates ConfigMap for env vars| Can add, modify, or remove variables |
| `images`                       | Image version for alist                    | Can modify `newTag` to update version|


### 2. ingress-patch.yaml  

Customizes the Ingress configuration for alist.


### 3. data-storage-patch.yaml  

Customizes `volumeClaimTemplates` in StatefulSet to configure alist's data storage.

### 4. local-storage-pvc-patch.yaml  

If additional local storage is required for alist, this file can be customized.


### 5. namespace.yaml  

Defines the namespace for alist resources.