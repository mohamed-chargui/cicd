pipeline{
    
    agent any 
    tools { 
        maven 'maven' 
    }
    stages {
        
        stage('Git Checkout'){
            
            steps{
                
                script{
                    
                    git branch: 'Develop', url: 'https://github.com/akremgr/CICD'
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
        stage('Static code analysis'){
            
            steps{
                
                script{
                    
                    withSonarQubeEnv(credentialsId: 'sonar-api') {
                        
                        sh 'mvn clean package sonar:sonar'
                    }
                   }
                    
                }
            }
        stage('Upload war file to nexus'){
            
            steps{
                
                script{
                    def pom = readMavenPom file: 'pom.xml'

                    pom_version_array = pom.version.split('\\.')

                    pom_version_array[1] = "${pom_version_array[1]}".toInteger() + 1

                    pom.version = pom_version_array.join('.')

                     writeMavenPom model: pom
                    nexusArtifactUploader artifacts:
                        [
                            [
                             artifactId: 'springboot',
                             classifier: '', file: 'target/Uber.jar', 
                             type: 'jar'
                            ]
                        ], 
                        credentialsId: 'nexus-auth', 
                        groupId: 'com.example', 
                        nexusUrl: 'localhost:8081', 
                        nexusVersion: 'nexus3', 
                        protocol: 'http', 
                        repository: 'DemoApp-Release', 
                        version: '${pom.version}'
                    
                   }
                    
                }
            }
        
        
        
        }
        
}
