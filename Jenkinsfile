pipeline{

    agent any
    tools {
        maven 'maven'
    }
    stages {

        stage('Git Checkout'){

            steps{

                script{

                    git branch: 'Develop', url: 'https://github.com/mohamed-chargui/cicd.git'
                }
            }
        }
        stage('Maven build'){

            steps{

                script{

                    sh 'mvn clean install'
                }
            }
        }
        stage('UNIT testing'){

            steps{

                script{

                    sh 'mvn test'
                }
            }
        }
stage('Integration testing'){

            steps{

                script{

                    sh 'mvn verify -DskipUnitTests'
                }
            }
        }
       
          stage('Static code analysis'){

            steps{

                script{

                    withSonarQubeEnv(credentialsId: 'tokenCI') {

                        sh 'mvn clean package sonar:sonar'
                    }
            }
         }
       }
          stage('nexus upload file .jar'){
            steps{
                script{
                       nexusArtifactUploader artifacts:
                        [
                            [
                                artifactId: 'springboot', 
                                classifier: '', 
                                file: 'target/uber.jar', 
                                type: 'jar'
                                ]
                         ], 
                                credentialsId: 'nexus_Auth', 
                                groupId: 'com.example', 
                                nexusUrl: '192.168.126.120:8081/', 
                                nexusVersion: 'nexus3', 
                                protocol: 'http', 
                                repository: 'democicd-repo', 
                                version: '2.0.0'
                }
               
            }
          }


    }
}

