pipeline {
    agent any
    parameters {
        string(name: 'username', description: 'Param 1?')
    }
    stages {
    stage('SCM Git Checkout'){
        steps {
        git 'https://github.com/sohitsrivastava/hello-world.git'
        }
    }
    stage('Compile Package'){
        steps {
            script {
        def mvnHome = tool name: 'Maven', type: 'maven'
        sh "${mvnHome}/bin/mvn package"
            }
        }
    }
    stage('Copy War File to Docker Folder'){ 
        steps {
        sh 'cp webapp/target/*.war ansadmin@172.31.12.1:/opt/docker'
        }
    }
    stage('Deploy to Docker'){ 
        steps {
        sh 'cd /opt/docker;ansible-playbook -i  hosts create-simple-devops-image.yml --limit localhost;ansible-playbook -i hosts create-simple-devops-project.yml --limit 172.31.0.125;'
        }
    }
   }
}
