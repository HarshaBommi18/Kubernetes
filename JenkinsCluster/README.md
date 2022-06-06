1. curl -fsSL https://get.docker.com -o get-docker.sh

2. sudo sh get-docker.sh

3. sudo usermod -aG docker $USER

4. curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

5. chmod +x kubectl
6. mv ./kubectl /usr/local/bin/kubectl
7. sudo mv ./kubectl /usr/local/bin/kubectl
8. kubectl version --client
9. curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.11.1/kind-linux-amd64

10. chmod +x ./kind
11. sudo mv ./kind /usr/local/bin
12. kind version
13. vim kind.yml >> 
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker

14. kind create cluster --name harsha --config kind.yml

15. kubectl create ns jenkins

16. kubectl apply -n jenkins -f rbac.yml

17. kubectl apply -n jenkins -f deploy.yml
18. kubectl apply -n jenkins -f svc.yml
19.  kubectport-forward svc/jenkins-master-service 8080:80 --address 0.0.0.0

or kubectl port-forward pod/jenkins-7cd98f5f86-qbl5j 8080 --address 0.0.0.0 -n jenkins

19. Now open the jenkins in the browser Ex: http://<ip>:8080 and configure the admin user settins

Note: to get the default password use the below command
         kubectl -n jenkins exec -it <podname> cat /var/jenkins_home/secrets/initialAdminPassword

20. Install the kubernetes plugin

21. Now go manage jenkins > Manage clouds and select kubernets in the dropdown

22. Fill the details as shown below

	Name: kubernetes
        Kubernetes URL: https://<Master Docker container IP>:443
        Kubernetes Namespace: jenkins
        Credentials | Add | Jenkins (Choose Kubernetes service account option & Global + Save)
        Test Connection | Should be successful! If not, check RBAC permissions and fix it!
        Jenkins URL: http://jenkins
        Tunnel : jenkins:50000

            Add Kubernetes Pod Template
            Name: jenkins-slave
            Namespace: jenkins
            Service Account: jenkins
            Labels: jenkins-slave (you will need to use this label on all jobs)
            Containers | Add Template
            Name: jnlp
            Docker Image: harshabommi/maven-jenkins:v2
            Command to run : empty
            Arguments to pass to the command: empty
            Allocate pseudo-TTY: yes

