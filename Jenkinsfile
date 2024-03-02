pipeline {
    agent any 

    stages {
        when {
            changeRequest()
        }
        stage('Git Checkout') {
            steps {
                git credentialsId: '5627ae37-58d5-4499-a716-2eeb98704359', url: 'https://github.com/aspaceincloud/IAC.git'
            }
        }

        stage('Terraform syntax check') {
            when {
            changeRequest()
        }
            steps{
                 dir("env/dev") {
                sh 'terraform fmt'
                 }
                }
        }
        stage('Initializing and Validation') {
            when {
            changeRequest()
        }
             steps {
                dir("env/dev") {
                sh 'terraform init'
                sh 'terraform validate'
                }
             }
        }

        stage('Terraform Plan') {
            when {
            changeRequest()
        }
            steps{
                dir("env/dev") {
                sh 'terraform plan -var-file=dev.tfvars' 
                }
            }
        }
        stage('Terraform Apply') {
           steps{
            dir("dev/env") {
                sh 'terraform apply -auto-approve -var-file=dev.tfvars'
            }
           }
        }

    }
 }

