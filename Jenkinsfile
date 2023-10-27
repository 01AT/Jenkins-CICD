@Library('Jenkins-Shared-Library') _
pipeline{
    agent any
    parameters{
        choice(name: 'action', choices: 'create/ndelete', description: 'Choose create or delete')
    }
    stages{

        when{ expression { param.action == 'create'}}
        stage('Git Checkout'){
            steps{
                gitCheckout(
                    branch: "main",
                    url: "https://github.com/01AT/Jenkins-CICD.git"
                )

            }
        }
        stage('Unit Test Maven'){
        when{ expression { param.action == 'create'}}    
            steps{
                script{
                    mvntest()
                }

            }
        }
        stage('Maven Integration Test'){
        when{ expression { param.action == 'create'}}
            steps{
                script{
                    mvnIntegrationTest()
                }

            }
        }
    }
}