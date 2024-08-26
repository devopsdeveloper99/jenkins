pipeline {
    agent any
    environment{
        AWS_REGION="us-west-2"
        THE_BUTLER_SAYS_SO=credentials('aws-creds')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'dev', url: 'https://github.com/devopsdeveloper99/jenkins.git' // Update with your Git repo
            }
        }

        stage('Terraform Init') {
            steps {
                script {
                        sh 'terraform init'
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                script {
                        sh 'terraform plan -out=tfplan'
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                script {

                        sh 'terraform apply tfplan'

                }
            }
        }
    }

    post {
        cleanup {
            script {
                echo "Cleaning up temporary files"
            }
        }
    }
}
