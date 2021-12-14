# Motivation
The following repo shows how to create a multi-node cluster from a simple 
config file in your local machine using a tool called **[kind](https://kind.sigs.k8s.io/docs/user/quick-start/)**
- Using the Cluster created by Kind we can then use the **[Helm](https://helm.sh/)** tool and **[Helm Chart](https://helm.sh/docs/topics/charts/)** to provision our applications in Kubernetes cluster environment.
- Helm Chart is basically is a unit of deployment, which made up of a set of YAML files.  
Example:
We can have a Helm Chart for a micro service or any software like Redis, MySQL, Prometheus, ELK Stack, etc.

- **An Helm Chart** has a name, it has an information file, and it has a template folder that contains all your YAMLs. It also has a values file containing the parameters and values we want to inject into the Chart and this can be the default configuration values.


## Creating Single node Kubernetes Cluster using Kind
```sh
$ kind create cluster --name my-cluster  # This will pull latest `kindest` image
or
$ kind create cluster --name my-cluster --image kindest/node:v1.19.1  # Specify specific kindest image
```
To check the cluster details use the following commands
```sh
$ kubectl get nodes
$ kubectl cluster-info --context kind-my-cluster
```

## Loading images from host or from archive into cluster
```sh
$ kind load --help
```

## Deleting clusters
```sh
$ kind delete cluster --name my-cluster
```

## Creating clusters through configs
```sh
$ kind create cluster --config .kind/multi-node.yaml
```

To verify
```sh
$ kubectl cluster-info --context kind-kind
$ kubectl get nodes
```

## Apply a set of configurations to run applications in the cluster
```sh
$ kubectl apply --filename k8s/
```

## You can also specify any GitHub absolute path for a config file
```sh
$ kubectl apply \
    --filename https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/kind/deploy.yaml
```

## Deleting clusters
```sh
$ kind delete cluster --name kind
```


## Create an Helm Chart
From above helper commands we have our kubernetes cluster ready for us to create our helm chart.

Following command will create a new helm chart
```sh
$ cd .helm
$ helm create example-app ./helm
```

## Verify the Helm Chart template is valid
```sh
$ helm template example-app ./helm/example-app
or 
$ cd .helm
$ helm template example-app example-app
```

## Install the Helm Chart
```sh
$ helm install example-app ./helm/example-app
or
$ cd .helm
$ helm install example-app example-app
```