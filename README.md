# Implement DevOps in Google Cloud: Challenge Lab

Here you can find the steps for solving the challenges in Devops challenge lab

### Required Setting 

We need to create all the resources in `us-east1` region and `us-east1-b` zone.

So, Open the cloud shell and paste the below code

```gcloud config set compute/zone us-east1-b```

### Step-2

#### Task 1: Configure a Jenkins pipeline for continuous deployment to Kubernetes Engine

### Step-1
  Clone the sample code for the lab
  > Remember replace `[ProjectID]` with your ProjectID
  
  ```git clone https://source.developers.google.com/p/[ProjectID]/r/sample-app```
  
### Step-2
  `jenkins-cd` cluster is already provisioned for the lab so don't need to create a new kubernetes cluster.
  You can verify that by running the following command
  
  ```gcloud container clusters list```
  
  Now get the credentials for the `jenkins-cd` cluster. 
  
  ```gcloud container clusters get-credentials jenkins-cd```
  
  Configure the Jenkins service account to be able to deploy to the cluster.
  
  ```kubectl create clusterrolebinding jenkins-deploy --clusterrole=cluster-admin --serviceaccount=default:cd-jenkins```
  
  Run the following command to setup port forwarding.
  
   ```
   export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/component=jenkins-master" -l 
   "app.kubernetes.io/instance=cd" -o jsonpath="{.items[0].metadata.name}")
   kubectl port-forward $POD_NAME 8080:8080 >> /dev/null &
   
   ```
   Get the password for the jenkins,run
   
   ```printf $(kubectl get secret cd-jenkins -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode);echo```
   
   Now open the Jenkins user interface by Click on **WebView** button on cloud shell and click on **Preview on port 8080**
   Jenkins will open in new tab. Your username is `admin` and enter the password you got by running the above command.
   
   Navigate to application directory
   
   ```cd sample-app```
   
   Create the Kubernetes namespace to logically isolate the deployment
   
   ```kubectl create ns production```
   
   Create the production and canary deployments, and the services using the `kubectl apply` commands
   
   ```kubectl apply -f k8s/production -n production```
   ```kubectl apply -f k8s/canary -n production```
   ```kubectl apply -f k8s/services -n production```
   
   Retrieve the external ip of the services, run
   
   ```kubectl get service gceme-frontend -n production```
   
   Paste the `external ip` you can see the info-card with blue background header
   
   Initialize the sample app directory
   
   ```git init```
   ```git config credential.helper gcloud.sh```
   ```git remote add origin https://source.developers.google.com/p/$DEVSHELL_PROJECT_ID/r/default```
   
   Set username and email
   
   ```git config --global user.email "[EMAIL_ADDRESS]"```
   > Replace `[EMAIL_ADDRESS]` with any git email(you can also replace with your GCP ProjectID)
   
   ```git config --global user.name "[USERNAME]"```
   
   > Replace `[EMAIL_ADDRESS]` with any git email(you can also replace with your GCP ProjectID)
   
   Next add your service account credentials for the jenkins to access code repository
   
   Go to jenkins as you have opened it in another tab by web preview
   
  
   
    
   
    
    
    
  
  




   
    
   
    
    
    
  
  



