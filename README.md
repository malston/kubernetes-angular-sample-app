# kubernetes-angular-sample-app

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 8.3.8.

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The app will automatically reload if you change any of the source files.

## Code scaffolding

Run `ng generate component component-name` to generate a new component. You can also use `ng generate directive|pipe|service|class|guard|interface|enum|module`.

## Build

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory. Use the `--prod` flag for a production build.

## Running unit tests

Run `ng test` to execute the unit tests via [Karma](https://karma-runner.github.io).

## Running end-to-end tests

Run `ng e2e` to execute the end-to-end tests via [Protractor](http://www.protractortest.org/).

## Further help

To get more help on the Angular CLI use `ng help` or go check out the [Angular CLI README](https://github.com/angular/angular-cli/blob/master/README.md).

## Docker

### Build the Docker image

```bash
docker_image=malston/spa-demo
docker build -t ${docker_image}:v1 .
docker push ${docker_image}:v1
```

Or if using Harbor

```bash
docker login "https://${HARBOR_HOST}" --username admin --password "${HARBOR_PASSWORD}"
docker tag "${docker_image}:v1" "${HARBOR_HOST}/library/${docker_image}:v1"
docker push "${HARBOR_HOST}/library/${docker_image}:v1"
```

## Kubernetes

### Create Namespace

```bash
kubectl create namespace spa-demo
```

### Create a Deployment

```bash
kubectl apply -f <(envsubst '${HARBOR_HOST}' <deployment.yaml)
```

### Create a Service

```bash
kubectl apply -f load-balancer-service.yaml
```

### Monitor the Deployment

```bash
watch 'kubectl get services,pods -o wide'
```

### Access the App

To access the app using a `LoadBalancer` type service:

```bash
EXTERNAL_LB_IP=$(kubectl get service spa-demo-loadbalancer -o jsonpath='{.status.loadBalancer.ingress[0].ip}')

curl $EXTERNAL_LB_IP
```
