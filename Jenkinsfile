@Library('Jenkins-Shared-Library') _
pipeline{
    agent any
    stages{
        stage("Git Checkout"){
            steps{
                script{
                    gitCheckout{
                        branch: 'main'
                        url: 'https://github.com/01AT/Jenkins-CICD.git'
                    }
    
                }

            }
        }
    }
}