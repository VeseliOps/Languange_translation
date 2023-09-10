@Library('shared-library') _
pipeline {
    agent any
    parameters {
        choice(name: 'action', choices: 'create\ndelete', description: 'Choose create/delete')
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
                   def SonarQubecredentialsId = 'sonarqube1-api'
                   statiCodeAnalysis(SonarQubecredentialsId)
                }
            }
        }
        stage('Quality Gate Status Check : Sonarqube'){
        when { expression { params.action == 'create' } }
            steps{
                script{
                   def SonarQubecredentialsId = 'sonarqube1-api'
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
    }
}
