pipeline {

agent any
	 environment {
        registry = "fatmabe/DevOps"
        registryCredential = 'dockerHub'
        dockerImage = ''
    }

stages {
	stage('Cloning our Git') { 
10
            steps { 
11
                git 'https://github.com/5se4-G1/DevOPs.git' 
12
            }
13
        } 



stage('Build Artifact - Maven') {
steps {
sh "mvn clean package -Dmaven.test.skip=true"
archive 'target/*.jar'
}
}
	stage("build & SonarQube analysis") {
            steps {
                sh 'mvn sonar:sonar -Dsonar.host.url=http://192.168.1.10:9000 -Dsonar.login=admin -Dsonar.password=sonar'
            }
	}
	
          stage('nexus'){
             steps{
                 sh 'mvn deploy -e '

             }
         }
	  
 stage('Building our image') { 
            steps { 
                script { 
                    dockerImage = docker.build registry + ":$BUILD_NUMBER" 
                }
            } 
        }

        stage('Deploy our image') { 
            steps { 
                script { 
                    docker.withRegistry( '', registryCredential ) { 
                        dockerImage.push() 
                   }
                } 
            }
        }

        stage('Cleaning up') { 
            steps { 
               sh "docker rmi $registry:$BUILD_NUMBER" 
            }
        } 
     }
}
