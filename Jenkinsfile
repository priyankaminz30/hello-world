pipeline {
    agent any
    parameters {
        string(name: 'username', description: 'Enter the username for the target server')
    }
    stages {
    stage('SCM Git Checkout'){
        steps {
        git 'https://github.com/sohitsrivastava/hello-world.git'
        }
    }
    stage('Compile Package'){
        agent {
  label 'slave1'
  }
        steps {
            script{
           sh 'mvn package'
            }
        }
    }
    stage('Deploy to Docker'){ 
        steps {
            echo "Deploying to target server with username: ${params.username}"
        sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible-server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''cd /opt/docker;
ansible-playbook -i  hosts create-simple-devops-image.yml --limit localhost;
ansible-playbook -i hosts create-simple-devops-project.yml --limit 172.31.0.125;''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '//opt//docker', remoteDirectorySDF: false, removePrefix: 'webapp/target', sourceFiles: 'webapp/target/*.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
        }
    }
   }
}
