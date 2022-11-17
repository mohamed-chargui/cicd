pipeline{
    
    agent any 
    tools { 
        maven 'maven' 
       stages {

        stage('Git Checkout'){

            steps{

                script{

                    git branch: 'Develop', url: 'https://github.com/mohamed-chargui/cicd.git'
                }
            }
        }
 
        
        }
        
}
}