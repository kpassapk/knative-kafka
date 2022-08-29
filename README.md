# Knative Kafka

## 1. Install requirements

- Docker
- Minikube
- Knative CLI

[Install][install] minikube and kn.

``` sh
brew install minikube
brew install kn
```

Ensure Docker is running. Then start minikube.

``` sh
minikube start --memory=4096
```

Make sure you are using minikube from kubectl:

```
kubectl config current-context
```

This should print `minikube`. Open the dashboard.

``` sh
minikube dashboard
```

Your dashboard should show four namespaces:
- default
- kube-node-lease
- kube-public
- kube-system
- kubernetes-dashboard

Make sure that all pods are in a "ready" state. (You can do this from the dashboard too.)

``` sh
kubectl get pods --all-namespaces
```

## 2. Install yaml files

Install the first 5 [yaml][with-yaml] files

``` sh
kubectl apply -f yamls/1_eventing-crds.yaml
kubectl apply -f yamls/2_eventing-core.yaml
kubectl apply -f yamls/3_eventing-kafka-controller.yaml
kubectl apply -f yamls/4_eventing-kafka-source.yaml
kubectl apply -f yamls/5_mt-channel-broker.yaml
```

[install]: https://knative.dev/docs/install/
[with-yaml]: https://knative.dev/docs/install/yaml-install/eventing/install-eventing-with-yaml/

## Install Kafka

``` sh
kubectl create namespace kafka
```

``` sh
kubectl create -f 'https://strimzi.io/install/latest?namespace=kafka' -n kafka
```

``` sh
kubectl apply -f https://strimzi.io/examples/latest/kafka/kafka-persistent-single.yaml -n kafka 
```

Watch the dashboard for stuff to turn green, or execute

``` sh
kubectl wait kafka/my-cluster --for=condition=Ready --timeout=300s -n kafka
```

## Link Kafka


Follow the remaining steps to install the [kafka source][kafka-source]:

``` sh
kubectl apply -f yamls/10_kafka_topic.yml
kubectl apply -f yamls/11_event_display.yaml
kubectl apply -f yamls/12_event_source.yaml
```

[kafka-source]: https://knative.dev/docs/eventing/sources/kafka-source/
[channel-defaults]: https://knative.dev/docs/eventing/configuration/kafka-channel-configuration/
