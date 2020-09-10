https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/

# Run & expose php-apache server

kubectl apply -f php-apache.yaml

## Create Horizontal Pod Autoscaler

kubectl autoscale deployment php-apache --cpu-percent=50 --min=1 --max=10

## We may check the current status of autoscaler by running

kubectl get hpa

# Increase load

kubectl run -it --rm load-generator --image=busybox /bin/sh
while true; do wget -q -O- http://php-apache; done

## Within a minute or so, we should see the higher CPU load by executing

kubectl get hpa

## Here, CPU consumption has increased to 305% of the request. As a result, the deployment was resized to 7 replicas:

kubectl get deployment php-apache

# Stop load

kubectl get hpa

kubectl get deployment php-apache
