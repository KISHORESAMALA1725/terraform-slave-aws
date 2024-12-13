pipeline {
    agent {
        label 'terraform-slave-aws'
    }
    parameters {
        string (name: 'USERNAME', defaultValue: 'KISHORE', description: 'Please enter your name')
        choice (name: 'terraform', choices: ["init","validate","plan","apply","destroy"], description: "this is terraform")
    }
    stages {
        stage ('this is terraform init stage') {
            when {
                expression {
                    params.terraform == "init"
                }
            }
            steps {
                script {
                    sh "terraform init"
                }
            }
        }
        stage ('This is Terraform validate: stage') {
            when {
                expression {
                    params.terraform == "validate"
                }
            }
            steps {
                script {
                    sh "terraform validate"
                }
            }
        }
        stage ('this is terraform plan') {
            when {
                expression {
                    params.terraform == "plan"
                }
            }
            steps {
                script {
                    sh "terraform plan"
                }
            }
        }
        stage ('This is terraform apply') {
            when {
                expression {
                    params.terraform == "apply"
                }
            }
            steps {
                script {
                    sh "terraform apply --auto-approve"
                }
            }
        }
        stage ('Terraform destory') {
            when {
                expression {
                    params.terraform == "destroy"
                }
            }
            steps {
                script {
                    sh "terraform destroy --auto-approve"
                }
            }
        }
    }
}
