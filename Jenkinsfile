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
                    pom = readMavenPom(file: 'pom.xml')
                    def pom_version = pom.version
                    nexusRepo = pom_version.endsWith("SNAPSHOT") ? "DemoApp-SNAPSHOT" : "DemoApp-Release"

                    
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
                        repository: nexusRepo , 
                        version: pom_version
                    
                   }
                    
                }
            }
        
        stage('Build Docker image'){
            
            steps{
                
                script{
                     
                        sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
                        sh 'docker image tag $JOB_NAME:v1.$BUILD_ID akremgr/$JOB_NAME:v1.$BUILD_ID'
                        sh 'docker image tag $JOB_NAME:v1.$BUILD_ID akremgr/$JOB_NAME:latest'
                   }
                    
                }
            }        
        
        }
        
}
