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
          stage ('quality gate '){
             steps {
               script{
                  waitForQualityGate abortPipeline: false, credentialsId: 'tokenCI'
            }
          }
         }


    }
}

