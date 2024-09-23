# Kubernetes Voting App

This demo repo is to document the steps needed to deploy a simple voting app on Kubernetes, and is primarily intended for personal reference. It is a point-in-time example and probably won't get updated much, if at all. It is largely based on the app built in the final lesson of the [Kubernetes Crash Course](https://www.youtube.com/watch?v=XuSQU5Grv1g), but I've made tweaks that I think are more appropriate.

## Prerequisites

1. Install `kubectl`
      ```
      brew install kubernetes-cli
      ```

      This is the primary CLI tool for interacting with Kubernetes.

2. Install `minikube`
      ```
      brew install minikube
      ```

      This will be used to test/experiment with Kubernetes locally.


## Building Out the Infrastructure

> ![NOTE]
> You can verify what's happening in the `minikube` dashboard.
> ```
> minikube dashboard
> ```
>
> You must keep your terminal open! Don't forget to set the namespace to `vote`.
>

1. Create the `redis-deployment`.
    ```
    kubectl create -f redis-deployment.yml
    ```

    This will be the data store for the `vote` app.

2. Create the `redis-service`.
    ```
    kubectl create -f redis-service.yml
    ```

    This creates the "proxy" that will allow the `vote` app to persist votes to the `redis-deployment`.

3. Create the `postgres-deployment`.
    ```
    kubectl create -f postgres-deployment.yml
    ```

    This will be the data source for the `results` app.

4. Create the `postgres-service`.
    ```
    kubectl create -f postgres-service.yml
    ```

    This creates the "proxy" that will allow the `results` app to retrieve votes from the `postgres-deployment`.

5. Create the `worker-deployment`.
    ```
    kubectl create -f worker-deployment.yml
    ```

    This will be the worker that will process votes from the `redis-deployment` and store them in the `postgres-deployment`.

6. Create the `results-deployment`.
    ```
    kubectl create -f results-deployment.yml
    ```

    This will be the `results` app that displays the vote results.

7. Create the `results-service`.
    ```
    kubectl create -f results-service.yml
    ```

    This creates the "proxy" that will allow us to access the `results` app. It will be exposed on port `31001`.

8. Access the `results` app. You can get the URL by running **in a fresh terminal window**:
    ```
    minikube service results-service --url -n vote
    ```

    This will open the `results` app in your default browser. **You will need to keep the terminal open.**

9. Create the `vote-deployment`.
    ```
    kubectl create -f vote-deployment.yml
    ```

    This will be the `vote` app that allows users to vote.

10. Create the `vote-service`.
    ```
    kubectl create -f vote-service.yml
    ```

    This creates the "proxy" that will allow us to access the `vote` app. It will be exposed on port `31000`.

11. Access the `vote` app. You can get the URL by running **in a fresh terminal**:
    ```
    minikube service vote-service --url -n vote
    ```

    This will open the `vote` app in your default browser. **You will need to keep the terminal open.**

12. Test the things!

### Clean Up

1. Close the `dashboard`, `vote`, and `results` app brower tabs.

2. Kill the terminals that were kept open to run them. (`CTRL+C`).

3. Shut down the `minikube` cluster.
    ```
    minikube stop
    ```

## Useful Commands

* Start `minikube`
    ```
    minikube start
    ```

* Open the `minikube` dashboard
    ```
    minikube dashboard
    ```

* Stop `minikube`
    ```
    minikube stop
    ```

* Delete `minikube` resources
    ```
    minikube delete
    ```

## Tips/Reminders

* These example files are namespaced to `vote`, so don't forget to account for that in your `kubectl` commands.
  * Example: `kubectl get po -n vote`