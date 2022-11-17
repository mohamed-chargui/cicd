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
       stage('Maven build'){

            steps{

                script{

                    sh 'mvn clean install'
                }
            }
        }

    }
} 