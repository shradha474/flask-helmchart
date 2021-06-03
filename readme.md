#How to setup Flask-app with jenkins and gitlab

Lets setup minikube first on your local system. please follow below url:-
https://minikube.sigs.k8s.io/docs/start/

Steps to setup gitlab and jenkins:

1. before moving forward with installation, lets install helm with below command:-
   https://helm.sh/docs/intro/install/

 after installtion, run "helm init" to initialize the helm charts

2. Now, we can deploy gitlab and jenkins using below commands.
   - helm install my-release bitnami/jenkins
   - helm install gitlab gitlab/charts --timeout 600s -f examples/values-minikube.yaml

Note: for setup gitlab clone the chart in local first and update the values-minikube.yaml accordingly.

PERSONAL NOTE: while doing setup for gitlab, the specification was not fullfil, and installation part was taking time. so i moved to virtual machine (RHEL 8) and did the installation.

3. Once Jenkins and gitlab is setup, lets create add credentails for sezzle@devops.com



Create the DOCKER image for flask-app:

1. Before moving forward with creating image lets setup docker by following below link:
   - https://docs.docker.com/get-docker/

2. Create a Dockerfile inside the flaskapp and add steps. you can follow the dockerfile present in repo.

NOTE: do not forget expose port in which application is running. for example: flask app is running on 4000 port

3. Now, lets create the image
   - docker build -t {{your-docker-user-id}}/flask-sezzle:1.2 . // here pass your docker userid

4. Once image is create, we can push it to the docker registery.

NOTE: make sure to run "docker login" command to login to docker registery.

   - docker push {{your-docker-user-id}}/flask-sezzle:1.2

Once image is push to docker-registery, we can move forward for creating helm chart for app

Create Helm chart for flask-app

1. run the following command:
   - helm create flaskapp

please follow the helm chart in the repository for value.yaml content.

2. Once helm chart is done we can deploy the flask app
   - helm install flaskapp flaskapp/ --values flaskapp/values.yaml

Now, lets automate this process with Gitlab and Jenkins

CONFIGURE CI/CD

Jenkins:
    - Install following packages:-
      - gitlab plugin
      - gitlab hook plugin

    - After restart go to manage-jenkins -> manage-credentials, add gitlab credentials

    - Go to manage-jenkins ->  configuration system and add the gitlab connections, (please follow the screenshots). you can also test the connection.

    - Create a project name "flask-deploy"
    - Add the gitlab repo with root credentials (Please follow screenshot)

Gitlab:
    - Create project name "sezzle-app"
    - go to project-settings --> integration --> jenkins --> now pass the jenkins url with username and password. (follow screenshot)

    - test the connection after saving

Once confgure is done we can move forward with setup deployemnt of application.

PERSONAL NOTE: because of the time limitation, i have done both step in same jenkins file.

Please follow the jenkinsfile in repo, for the setup and trigger the pipeline.

update the flask_demo code and dockerfile accordingly along with required chart changes. deploy it.




VOILA, Now you have complete setup for the flask deployment.

THANK YOU
