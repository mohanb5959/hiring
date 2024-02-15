pipeline {
    agent any
    stages {
        
        stage('Maven  Build') {
           steps {
                sh "mvn clean package"
            }
        }
        stage('Docker Build') {
            steps {
                sh "docker build -t mohan99061/hiring:0.0.2 ." 
                }
            }
        stage('Docker Push') {
           steps {
               withCredentials([string(credentialsId: 'docker-hub', variable: 'hubPwd')]) {
                   sh "docker login -u mohan99061 -p ${hubPwd}"
                   sh "docker push mohan99061/hiring:0.0.2"
               }
           }
       }
        stage('Docker Deploy'){
            steps {
                sshagent(['docker-host']) {
                    sh "ssh ec2-user@172.31.86.84 docker run -d -p 8080:8080 --name hiring mohan99061/hiring:0.0.2"
                }
            }
        }
    }
}
