pipeline {
    agent any
    environment {
        AWS_REGION = 'us-east-2'
    }
    parameters {
        choice(name: 'ACTION', choices: ['apply', 'destroy'], description: 'Select action: apply or destroy')
    }
    stages {
       stage('Checkout Repositories') {
            steps {
                script {
                        git branch: 'main', credentialsId: 'git_credentials', url: 'https://github.com/AnnemAnitha7989/TERRAFORM-.git'
                        git branch: 'main', credentialsId: 'git_credentials', url: 'https://github.com/AnnemAnitha7989/Ansible-role.git
                        }
                   }
         }

        stage('Terraform Init') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'aws-credentials', 
                                                       usernameVariable: 'AWS_ACCESS_KEY_ID', 
                                                       passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                        // Change to the my-tool-terraform-code directory where Terraform files are located
                        dir('my-tool-terraform-code') {
                            sh 'terraform init'
                        }
                    }
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'aws-credentials', 
                                                       usernameVariable: 'AWS_ACCESS_KEY_ID', 
                                                       passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                        // Run terraform plan in the my-tool-terraform-code directory
                        dir('my-tool-terraform-code') {
                            sh 'terraform plan'
                        }
                    }
                }
            }
        }
        stage('Approval For Apply') {
            when {
                expression { params.ACTION == 'apply' }
            }
            steps {
                // Prompt for approval before applying changes
                input "Do you want to apply Terraform changes?"
            }
        }

        stage('Terraform Apply') {
            when {
                expression { params.ACTION == 'apply' }
            }
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'aws-credentials', 
                                                       usernameVariable: 'AWS_ACCESS_KEY_ID', 
                                                       passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                        // Run terraform apply in the my-tool-terraform-code directory
                        dir('my-tool-terraform-code') {
                            sh 'terraform apply -auto-approve'
                        }
                    }
                }
            }
        }
        stage('Approval for Destroy') {
            when {
                expression { params.ACTION == 'destroy' }
            }
            steps {
                // Prompt for approval before destroying resources
                input "Do you want to Terraform Destroy?"
            }
        }

        stage('Terraform Destroy') {
            when {
                expression { params.ACTION == 'destroy' }
            }
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'aws-credentials', 
                                                       usernameVariable: 'AWS_ACCESS_KEY_ID', 
                                                       passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                        // Run terraform destroy in the my-tool-terraform-code directory
                        dir('my-tool-terraform-code') {
                            sh 'terraform destroy -auto-approve'
                        }
                    }
                }
            }
        }
    }
}
