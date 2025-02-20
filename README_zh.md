# alist Kustomize 配置仓库

该仓库用于通过 [Kustomize](https://kustomize.io/) 方便的在Kubernetes上部署 [alist](https://alist.nn.ci) 应用。

## 使用方法

首先，将当前仓库克隆到本地：
``` bash
git clone https://github.com/sujoshua/alist-kustomize.git
cd alist-kustomize
```
进入 overlay 目录，其中包含不同环境的配置模板：
``` bash
cd overlay
```

将 example 目录复制一份，并重命名为 prod（你也可以根据环境需求改为其他名字，如 dev、staging 等）：
``` bash
cp -r example prod
cd prod
```

根据你的需求，修改 prod 目录下的配置文件。具体可修改的内容，请参考下方 [配置项说明](#配置说明) 部分。


在 prod 目录下，可以通过以下命令预览生成的 YAML 配置：
```bash
kustomize build .
```

或者通过 kubectl 进行 dry-run：
```bash
kubectl apply -k . --dry-run=client
```
确认配置无误后，通过下面命令应用配置到 Kubernetes 集群：
```bash
kubectl apply -k .
```

## 配置说明

### 1. kustomization.yaml
`kustomization.yaml` 是 Kustomize 的主配置文件，用于定义资源、生成 ConfigMap、镜像版本、Patch 等内容。

| 配置项                    | 功能说明                          | 可修改内容                       |
|---------------------------|-----------------------------------|----------------------------------|
| `namespace`               | 所有资源所属的命名空间             | 可以修改为自定义命名空间名称      |
| `namePrefix`              | 为所有资源添加前缀                 | 可以修改或移除前缀                |
| `labels`                  | 为所有资源添加通用标签             | 可根据需求添加或修改标签内容      |
| `configMapGenerator.env`      | 动态生成 ConfigMap 传递环境变量    | 可增加、修改或删除环境变量         |
| `images`                  | 镜像版本                          | 可修改 `newTag` 更新 alist 版本   |

---

### 2. ingress-patch.yaml

用于自定义 Ingress 配置。

### 3. data-storage-patch.yaml

用于自定义 StatefulSet 中的 `volumeClaimTemplates`，以配置alist的数据存储。


### 4. local-storage-pvc-patch.yaml

如果需要为 alist 提供额外的本地存储，可以使用此配置文件进行定制。


### 5. namespace.yaml

定义 alist 所属的命名空间。
