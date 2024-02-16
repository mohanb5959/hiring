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
                sh "docker build . -t mohan99061/hiring:${commit_id()}" 
                }
            }
        stage('Docker Push') {
           steps {
               withCredentials([string(credentialsId: 'docker-hub', variable: 'hubPwd')]) {
                   sh "docker login -u mohan99061 -p ${hubPwd}"
                   sh "docker push mohan99061/hiring:${commit_id()}"
               }
           }
       }
        stage('Docker Deploy'){
            steps {
                sshagent(['docker-host']) {
                    sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.86.84 docker rm -f hiring"
                    sh "ssh ec2-user@172.31.86.84 docker run -d -p 8080:8080 --name hiring mohan99061/hiring:${commit_id()}"
                }
            }
        }
    }
}


def commit_id(){
    id = sh returnStdout: true, script: 'git rev-parse HEAD'
    return id
}
