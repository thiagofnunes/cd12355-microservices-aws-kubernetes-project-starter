1. Configure AWS Credentials (If it expires)
aws configure

2. Validate that the credentials are ok
aws sts get-caller-identity

3. Generate the cluster with a single node group
eksctl create cluster --name my-project-cluster --region us-east-1 --nodegroup-name my-project-nodes --node-type t3.small --nodes 1 --nodes-min 1 --nodes-max 2

4. Update the kubectl to point to the generated cluster (Add context)
aws eks --region us-east-1 update-kubeconfig --name my-project-cluster

5.Validate that it is using the correct context
kubectl config current-context

6. Validate the storage class
kubectl get storageclass

7. Apply the YAML configuration

kubectl apply -f pvc.yml
kubectl apply -f pv.yml
kubectl apply -f postgresql-deployment.yml

8. Validate that there is a new pod properly created and running
kubectl get pods

9. Apply the service YAML, to be able to connect to the database remotely
kubectl apply -f postgresql-service.yml

10. Validate that the service was created and check the IP
kubectl get svc

11. Set up port forwarding
kubectl port-forward service/postgresql-service 5433:5432 &

12. Apply the SQL scripts to the database
I used Datagrip

13. Set up port forwarding
kubectl port-forward --namespace default svc/<SERVICE_NAME>-postgresql 5433:5432 &

14. Export the password
export POSTGRES_PASSWORD=$(kubectl get secret --namespace default <SERVICE_NAME>-postgresql -o jsonpath="{.data.postgres-password}" | base64 -d)

15. Create the repository in CodeBuild, connect it to GitHub and create the action to send it to ECR


16. Apply the configmap and service YAMLs

kubectl apply -f configmap.yaml
kubectl apply -f coworking.yaml

17. Validate that everything is ok

kubectl get svc
kubectl get pods

18. Attach the CloudWatch policy so it can gather logs
aws iam attach-role-policy --role-name arn:aws:iam::782437785034:role/eksctl-my-project-cluster-nodegrou-NodeInstanceRole-bbTFm1a2cLvN --policy-arn arn:aws:iam::aws:policy/CloudWatchAgentServerPolicy
aws eks create-addon --addon-name amazon-cloudwatch-observability --cluster-name my-project-cluster

19. You are good to go 😉😊