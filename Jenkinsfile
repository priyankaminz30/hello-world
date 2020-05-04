import org.jenkinsci.plugins.pipeline.modeldefinition.Utils
pipeline {
    agent any
    
    parameters {
        string(name: 'username', description: 'Enter the username for the target server')
        booleanParam(name: 'IIB', defaultValue: true, description: 'Extract for IIB') 
        booleanParam(name: 'WAS', defaultValue: true, description: 'Extract for WAS') 
        booleanParam(name: 'J2SE', defaultValue: true, description: 'Extract for J2SE') 
		booleanParam(name: 'ALL', defaultValue: true, description: 'Extract for ALL')
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
        stage('Archive Artefacts'){
            steps{
                archiveArtifacts '**/*.war'
            }
        }
	stage('Check Username'){
		when{
		   environment  name: 'username' , value : 'Sohit'
		 }
		 steps{
			echo "username verified as sohit"
		 }
	}
	stage('Extract IIB'){
		when{
		   environment  name: 'username' , value : 'Sohit'
		   anyOf {
                    environment name: 'ALL', value: 'true'
                    environment name: 'IIB', value: 'true'
                }
		 }
		 steps{
			echo "Extracting logs for IIB"
		 }
	}
	stage('Extract WAS'){
		when{
		   environment  name: 'username' , value : 'Sohit'
		   anyOf {
                    environment name: 'ALL', value: 'true'
                    environment name: 'WAS', value: 'true'
                }
		 }
		 steps{
			echo "Extracting logs for WAS"
		 }
	}
	stage('Extract J2SE'){
		when{
		   environment  name: 'username' , value : 'Sohit'
		   anyOf {
                    environment name: 'ALL', value: 'true'
                    environment name: 'J2SE', value: 'true'
                }
		 }
		 steps{
			echo "Extracting logs for J2SE"
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
