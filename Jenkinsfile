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
            script{
           sh '/opt/maven/bin/mvn package'
            }
        }
    }
    stage('Deploy to Docker'){ 
        steps {
        sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible-server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''cd /opt/docker;
ansible-playbook -i  hosts create-simple-devops-image.yml --limit localhost;
ansible-playbook -i hosts create-simple-devops-project.yml --limit 172.31.0.125;''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '//opt//docker', remoteDirectorySDF: false, removePrefix: 'webapp/target', sourceFiles: 'webapp/target/*.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
        }
    }
   }
}
