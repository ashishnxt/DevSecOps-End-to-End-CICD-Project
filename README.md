
• AWS EC2 instance (Ubuntu) with instance type t2.large and root volume 29GB.


• Jenkins installed
    Reference:Jenkins installation (jenkins.io/doc/book/installing/linux/#long-term-support-release)
     first need to install java because Jenkins made on java
     sudo apt update
     sudo apt install fontconfig openjdk-17-jre


---------------------------------Jenkins installation----------------------------------------------

sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
/etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install Jenkins

* Jenkins by default work on 8080 port
for check it's running or not : systemctl status Jenkins

* by default Jenkins is locked 
  for unlock Jenkins open file.
  sudo cat /var/lib/jenkins/secrets/initialAdminPassword



• Jenkins Plugins Installation:
   Go to Manage Jenkins, click on Plugins and install all the plugins listed below, we will require for other tools integration:

   •SonarQube Scanner (Version2.16.1)  
   •Sonar Quality Gates (Version1.3.1)  
   •OWASP Dependency-Check (Version5.4.3)
   •Docker (Version1.5)


   Pipeline are two types:
   •static
   •declarative (in this pipeline stages are declared) 


  groovy script :

  pipeline {
    agent any
    environment {
    SONAR_PIPE = tool "sonar"
  }
    stages {
        stage("code") {
            steps {
               
                    echo "this is awesome"
                
            }
        }
        stage("test") {
            steps {
                
                    echo "this is also awesome"
                    // Add more test-related steps here
             
            }
        }
        stage("build") {
            steps {
              
                    echo "building..."
                    // Add build-related steps here
              
            }
        }
        stage("scan") {
            steps {
               
                    echo "scanning..."
                    // Add scan-related steps here
                
            }
        }
        stage("deploy") {
            steps {  
              
                    echo "deploying..."
                    // Add deployment-related steps here
            }
         }
      }
   }



----------------------------------------------------------------------------------------------------------------






Parsing file /var/lib/jenkins/workspace/wanderlust-ci-cd/dependency-check-report.xmt

  •because Jenkins have not permission to access docker compose for deploy the app.
   add Jenkins user in the docker group for access the docker 

    sudo usermod -aG docker Jenkins
    sudo systemctl restart docker
    sudo systemctl restart Jenkins





------------------------------------------- SonarQube installation: -----------------------------------------------

   SonarQube run on docker on port 9000

   cmd: docker run —itd --name sonarqube-server -p 9000:9000 sonarqube:lts-community
                 ( -itd : interactive and deattach mode means work in the background)
                 -p : run on 9000 port , name SonarQube lts latest version 
   docker ps (for check sonar is run or not on port 9000)


   for start the SonarQube after restart the system.

    cmd : docker ps -a (which container is stop)
    cmd: docker start <container id>
   
    Default SonarQube user name : admin
                       password : admin

    generate  token: squ_3aa63175f8414f4744bac72f87afc025ee8d6d00


------------------------------------------------- Trivy installed --------------------------------------------------- 

   reference: (https://aquasecurity.github.io/trivy/v0.18.3/installation/)

   sudo apt-get install wget apt-transport-https gnupg lsb-release
   wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
   echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a /etc/apt/sources.list.d/trivy.list
   sudo apt-get update
   sudo apt-get install trivy


   trivy image imagename
   trivy fjs --security-checks vuln,config Folder-name-OR=Path



-----------------------------------------Docker and docker-compose installed-------------------------------------------------

  sudo apt—get-Update
  sudo apt—get install docker.io —y
  sudo apt-get install docker-compose -y


  •for check docker is install

  docker ps
  whoami
  by default user have to permission to access docker
  sudo usermod -aG docker $USER
  sudo reboot
  docker ps 



  The project is deploy on port :5173