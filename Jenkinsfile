pipeline{
    agent any
    tools {
        maven 'Default Maven'
    }
    stages{
        stage('Build'){
            steps{
                sh 'mvn clean package'
            }
            post{
                success{
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deploy to stage'){
            steps {
                build job: 'deploy-to-staging'
            }
        }

        stage('Deploy to Production'){
            steps {
                timeout(time:5, unit:'DAYS'){
                    input message: 'Approve PRODUCTION Deployment?'
                }

                build job: 'deploy-to-prod'
            }
            post {
                success {
                    echo 'Code deployed to production'
                }

                failure {
                    echo 'Deployment failed'
                }
            }
        }
    }
}