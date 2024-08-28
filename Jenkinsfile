pipeline {

    parameters {
        booleanParam(name: 'autoApprove', defaultValue: false, description: 'Automatically run apply after generating plan?')
    } 
    environment{
        AWS_REGION="us-west-2"
        THE_BUTLER_SAYS_SO=credentials('aws-creds')
    }

   agent  any

    stages {
        stage('Checkout Code') {
            steps {
                // Checkout the code from the repository
                checkout scm
            }
        }

        stage('Terraform Init') {
            steps {
                // Navigate to the Terraform directory and initialize
                dir('terraform') {
                    sh 'terraform init'
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                // Plan the infrastructure changes
                dir('terraform') {
                    sh 'terraform plan -out=tfplan'
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                // Apply the changes to deploy the EC2 instance
                dir('terraform') {
                    sh 'terraform apply -auto-approve tfplan'
                }
            }
        }
        stage('Terraform Destroy') {
            when {
                expression {
                    return params.TERRAFORM_ACTION == 'destroy'
                }
            }
            steps {
                // Destroy the infrastructure
                dir('terraform') {
                    sh 'terraform destroy -auto-approve'
                }
            }
        }
    }

    post {
        always {
            // Cleanup, notifications, or other post-build actions
            echo 'Pipeline completed.'
        }
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
    parameters {
        choice(
            name: 'TERRAFORM_ACTION',
            choices: ['apply', 'destroy'],
            description: 'Choose whether to apply or destroy Terraform resources'
        )
    }
}
