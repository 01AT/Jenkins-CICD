@Library('Jenkins_shared_Library') _
pipeline{
    agent any
    parameters{
        choice(name: 'action', choices: 'create\ndelete', description: 'Choose create/Destroy')
        string(name: 'ImageName', description: "name of the docker build", defaultValue: 'javapp')
        string(name: 'ImageTag', description: "tag of the docker build", defaultValue: 'v1')
        string(name: 'DockerHubUser', description: "name of the Application", defaultValue: 'abhiayan')
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
                    def SonarQubeCredentialsId = 'sonarqube-api'
                    staticcodeanalysis(SonarQubeCredentialsId)
                }

            }
        }
        stage('Quality Gate Status Check : SonarQube'){
            when{ expression { params.action == 'create'}}
            steps{
                script{
                    def SonarQubeCredentialsId = 'sonarqube-api'
                    QualityGateStatus(SonarQubeCredentialsId)
                }

            }
        }
        stage('maven build'){
            when{ expression { params.action == 'create'}}
            steps{
                script{
                    mavenbuild()
                }

            }
        }
        stage('Docker Image Build'){
            when{ expression { params.action == 'create'}}
            steps{
                script{
                    dockerbuild("${params.ImageName}", "${params.ImageTag}", "${params.DockerHubUser}")
                }

            }
        }
        stage('Docker Image Scan: trivy'){
            when{ expression { params.action == 'create'}}
            steps{
                script{
                    dockerscanimage("${params.ImageName}", "${params.ImageTag}", "${params.DockerHubUser}")
                }

            }
        }
        stage('Docker Image Push : DockerHub '){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   dockerimagepush("${params.ImageName}","${params.ImageTag}","${params.DockerHubUser}")
               }
            }
        }
        stage('Docker Image Cleanup : DockerHub '){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   dockerimagecleanup("${params.ImageName}","${params.ImageTag}","${params.DockerHubUser}")
               }
            }
        }
    }
}
