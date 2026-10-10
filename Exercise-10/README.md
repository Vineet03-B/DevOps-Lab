# Exercise 10: Multi-Node Kubernetes Cluster with Multiple Applications and Replicasets

## Objective
Deploy two Flask applications on a three-node Minikube cluster: a Product Catalog with two replicas and a Shopping Cart with three replicas. Use Docker images, Kubernetes Deployments, Services, and pod anti-affinity to distribute replicas across nodes.

## Project structure

```text
Exercise-10/
├── README.md
├── Dockerfile.product
├── Dockerfile.shopping
├── product_catalog.py
├── shopping_cart.py
├── product_catalog_deployment.yaml
├── shopping_cart_deployment.yaml
├── product_catalog_service.yaml
├── shopping_cart_service.yaml
└── screenshots/
```

## Applications

### Product Catalog
`product_catalog.py` is a Flask API that exposes `GET /products` and returns three sample products: Laptop, Phone, and Headphones.

### Shopping Cart
`shopping_cart.py` is a Flask API that exposes `GET /cart` to retrieve the in-memory cart and `POST /cart` to add a JSON item. The cart is in memory, so its contents are not persistent across pod restarts and are not shared between replicas.

## Docker images

`Dockerfile.product` builds the Product Catalog image and `Dockerfile.shopping` builds the Shopping Cart image. Both use `python:3.9-slim`, install Flask, and run the corresponding application on port 80.

From the directory containing the files, build the images:

```bash
docker build -t product-catalog:latest -f Dockerfile.product .
docker build -t shopping-cart:latest -f Dockerfile.shopping .
```

Load the images into the Minikube profile so the cluster can use them:

```bash
minikube -p devops-multinode image load product-catalog:latest
minikube -p devops-multinode image load shopping-cart:latest
```

## Kubernetes setup

Start a three-node Minikube cluster and enable the registry add-on if required by your setup:

```bash
minikube start --nodes 3 -p devops-multinode --force
minikube -p devops-multinode addons enable registry
```

Create the namespace used by the Shopping Cart deployment, then apply the Deployments and Services:

```bash
kubectl create namespace devops-exercise
kubectl apply -f product_catalog_deployment.yaml
kubectl apply -f shopping_cart_deployment.yaml
kubectl apply -f product_catalog_service.yaml
kubectl apply -f shopping_cart_service.yaml
```

The Product Catalog Deployment runs two replicas in the `default` namespace. The Shopping Cart Deployment runs three replicas in the `devops-exercise` namespace. Both Deployment manifests configure required pod anti-affinity using `kubernetes.io/hostname`, which asks Kubernetes to place matching replicas on different nodes. This requires enough eligible nodes and available resources.

Both Services use `NodePort` type and select their application pods by label.

## Verification

Check pod status and node placement:

```bash
kubectl get pods -A -o wide
kubectl get deployments -A
kubectl get services -A
```

Access each service in a separate terminal, leaving the terminal running while Minikube maintains its service tunnel:

```bash
minikube -p devops-multinode service product-catalog-service -n default
minikube -p devops-multinode service shopping-cart-service -n devops-exercise
```

Use the local forwarded URL reported by Minikube for API tests. Replace `<PRODUCT_URL>` and `<CART_URL>` with the corresponding URLs:

```bash
curl <PRODUCT_URL>/products
curl <CART_URL>/cart
curl -X POST <CART_URL>/cart -H "Content-Type: application/json" -d '{"id": 1, "name": "Laptop", "quantity": 1}'
curl <CART_URL>/cart
```

The product endpoint should return the sample product list. The cart endpoint initially returns an empty list; a successful POST returns the submitted item in that pod's in-memory cart. Because the application uses in-memory state and multiple replicas, a later request routed to another replica may return a different cart state.

## Results

The supplied screenshots document Docker image builds and image availability, Kubernetes manifest creation/application, pod distribution across nodes, service access, and API responses. Use the actual output from the current cluster when verifying the deployment, since service-forwarding ports and pod names can change between runs.
