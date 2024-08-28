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
        stage('checkout') {
            steps {
                 script{
                        dir("tfcode")
                        {
                            git "https://github.com/devopsdeveloper99/jenkins.git"
                        }
                    }
                }
            }

        stage('Plan') {
            steps {
                sh 'pwd;cd tfcode/ ; tfcode init'
                sh "pwd;cd tfcode/ ; tfcode plan -out tfplan"
                sh 'pwd;cd tfcode/ ; tfcode show -no-color tfplan > tfplan.txt'
            }
        }
        stage('Approval') {
           when {
               not {
                   equals expected: true, actual: params.autoApprove
               }
           }

           steps {
               script {
                    def plan = readFile 'tfcode/tfplan.txt'
                    input message: "Do you want to apply the plan?",
                    parameters: [text(name: 'Plan', description: 'Please review the plan', defaultValue: plan)]
               }
           }
       }

        stage('Apply') {
            steps {
                sh "pwd;cd tfcode/ ; tfcode apply -input=false tfplan"
            }
        }
    }

  }