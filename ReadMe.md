### Sample CodeIgniter Project

## How to run

- `docker-compose up --build`

## Configuring and deploying into local kubernetes using `microk8s`

- Install [microk8s](https://microk8s.io/)

- Enable registry `microk8s enable registry`

- Do `multipass list`, and take note of the `microk8s-vm` IP

- Now we have to use the IP. In `/etc/hosts` file, add this entry: `{IP} microk8s.multipass.org`

- Add `microk8s.multipass.org:32000` as insecure registry to your docker, following instructions given [here](https://docs.docker.com/registry/insecure/)

- Finally build your image and push like below:
  
  ```
  docker build . -t microk8s.multipass.org:32000/ci-app:1.0.0 -t localhost:32000/ci-app:1.0.0
  docker push microk8s.multipass.org:32000/ci-app:1.0.0
  ```

- Apply the `kubernetes` namespace: `microk8s kubectl apply -f k8s/namespace.yaml`

- Apply `deployment` and `service`: `microk8s kubectl apply -f k8s`

- Now port forward the created service to test out: `microk8s kubectl port-forward service/ci-app 8080:80`; you can hit `localhost:8080` to see the service