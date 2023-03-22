pipeline {
    agent any
    tools {
        maven 'v3.6.1'
    }

    stages {
        stage('git') {
            steps {
                git branch: 'main', url: 'https://github.com/Varun-TM/Jenkins-tast2.git'
            }
        }  
        stage('package') {
            steps {
                sh ' mvn package'
            }
        }
        stage ('deploy'){
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'petclinic', keyFileVariable: 'SSHKEY', usernameVariable: 'USERNAME')]) {
                    sh 'rsync -avzP -e "ssh -o StrictHostKeyChecking=no -i $SSHKEY" target/spring-petclinic-2.3.1.BUILD-SNAPSHOT.jar $USERNAME@43.205.229.246:/home/deploy/app/'
                    sh 'ssh -o StrictHostKeyChecking=no -i $SSHKEY $USERNAME@43.205.229.246 sudo /usr/bin/systemctl restart petclinic.service'
                    sh 'ssh -o StrictHostKeyChecking=no -i $SSHKEY $USERNAME@43.205.229.246 sudo /usr/bin/systemctl status petclinic.service'
                }
            }
        }
    }
}

