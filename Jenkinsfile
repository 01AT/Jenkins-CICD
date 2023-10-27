@Library('Jenkins-Shared-Library') _
pipeline{
    agent any
    parameters{
        choice(name: 'action', choices: 'create\ndelete', description: 'Choose create or delete')
    }
    stages{
        stage('Git Checkout'){
            when{ expression { params.action == 'create'}}
            steps{
                gitCheckout(
                    branch: "main",
                    url: "https://github.com/01AT/Jenkins-CICD.git"
                )

            }
        }
        stage('Unit Test Maven'){
            when{ expression { params.action == 'create'}}    
            steps{
                script{
                    mvntest()
                }

            }
        }
        stage('Maven Integration Test'){
            when{ expression { params.action == 'create'}}
            steps{
                script{
                    mvnIntegrationTest()
                }

            }
        }
        stage('Static Code Analysis'){
            when{ expression { params.action == 'create'}}
            steps{
                script{
                    def SonarQubeCredentialsId = 'sonar-api'
                    staticcodeanalysis(SonarQubeCredentialsId)
                }

            }
        }
        stage('Quality Gate Status Check : SonarQube'){
            when{ expression { params.action == 'create'}}
            steps{
                script{
                    def SonarQubeCredentialsId = 'sonar-api'
                    QualityGateStatus(SonarQubeCredentialsId)
                }

            }
        }
        stage('maven build'){
            when{ expression { params.action == 'create'}}
            steps{
                script{
                    mvnbuild()
                }

            }
        }
    }
}