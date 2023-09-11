@Library('shared-library') _
pipeline {
    agent any
    parameters {
        choice(name: 'action', choices: 'create\ndelete', description: 'Choose create/delete')
        string(name: 'ImageName', description: "name of the docker build", defaultValue: 'javapp')
        string(name: 'ImageTag', description: "tag of the docker build", defaultValue: 'v1')
        string(name: 'DockerHubUser', description: "name of the Application", defaultValue: 'aveselinovic')
    }
    stages {
        stage('Git Checkout'){
         when { expression { params.action == 'create' } }
            steps{
                script{
                    gitCheckout(
                        branch: "main",
                        url: "https://github.com/VeseliOpss/Languange_translation.git"
                        )
                }
            }
        }
        stage('Unit test maven'){
        when { expression { params.action == 'create' } }
            steps{
                script{
                    mvnTest()
                }
            }
        }
        stage('Integration test'){
        when { expression { params.action == 'create' } }
            steps{
                script{
                    mvnIntegrationTest()
                }
            }
        }
        stage('Sonarqube analysis'){
        when { expression { params.action == 'create' } }
            steps{
                script{
                   def SonarQubecredentialsId = 'veseli-api'
                   statiCodeAnalysis(SonarQubecredentialsId)
                }
            }
        }
        stage('Quality Gate Status Check : Sonarqube'){
        when { expression { params.action == 'create' } }
            steps{
                script{
                   def SonarQubecredentialsId = 'veseli-api'
                   QualityGateStatus(SonarQubecredentialsId)
                }
            }
        }
        stage('Maven build'){
        when { expression { params.action == 'create' } }
            steps{
                script{
                   mvnBuild()
                }
            }
        }
        stage('Build Docker image'){
        when { expression { params.action == 'create' } }
            steps{
                script{
                    
                  dockerBuild("${params.ImageName}","${params.ImageTag}","${params.DockerHubUser}")
                }
            }
        }
        stage('Scan Docker Image'){
        when { expression { params.action == 'create' } }
            steps{
                script{
                    
                  dockerImageScan("${params.ImageName}","${params.ImageTag}","${params.DockerHubUser}")
                }
            }
        }
    }
}
