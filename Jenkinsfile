import java.text.SimpleDateFormat
pipeline {
    agent any

     environment {
            registry = "fatma/dproject"
            registryCredential = 'dckr_pat_NX_qTIaloGguDSY22Ki8Jk04CJo'
            dockerImage = ''
     }

    stages {

        stage('Checkout GIT') {
            steps {
                echo 'Pulling...';
                git branch: 'fatma',
                url : 'https://github.com/5se4-G1/DevOPs.git'
            }
        }

        stage('Date') {
                steps {
                     script{
                     def date = new Date()
                     sdf = new SimpleDateFormat("MM/dd/yyyy")
                     println(sdf.format(date))
                             }
                             }
                             }

        stage('MVN CLEAN'){
            steps{
                sh  'mvn clean'
            }
        }

        stage('MVN COMPILE'){
            steps{
                sh  'mvn compile'
            }
        }

        stage('MVN PACKAGE'){
              steps{
                  sh  'mvn package'
              }
        }

        stage('Start Containers with Ansible'){
               steps{
                          sh  'ansible-playbook  ansible-playbook.yml'
               }

         }

        stage('Building our image') {
               steps{
                        script {
                            dockerImage = docker.build registry + ":$BUILD_NUMBER"
                        }
               }
        }

        stage('Docker images'){
               steps{
                        sh 'docker images'
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

          stage('DOCKER COMPOSE') {
                steps {
                            sh 'docker-compose up -d --build'
                }
          }

          stage("nexus deploy"){
               steps{
                       sh 'mvn  deploy'
               }
          }

          stage('MVN SONARQUBE'){

                steps{
                          sh  'mvn sonar:sonar -Dsonar.login=admin -Dsonar.password=sonar'
                }
          }
          stage("Test JUnit /Mockito"){
                steps {
                            sh 'mvn test'
                }
          }

    

    
        }
}



