# terraform-pipeline

<img width="1910" height="331" alt="image" src="https://github.com/user-attachments/assets/cc823e2e-08a3-46d9-9402-b2a56ef66a13" />

---
### Install Jenkins and Terraform
````
https://www.jenkins.io/doc/book/installing/linux/#debianubuntu
````
````
https://developer.hashicorp.com/terraform/install
````

---
### Install Plugins
````
 pipeline stage view
````
````
terraform
````
````
aws credentials
````

---
### provide aws credentials

<img width="1915" height="422" alt="image" src="https://github.com/user-attachments/assets/782ab612-0b23-4e55-822f-3029b20b8874" />


### setup jenkins pipeline

````
pipeline {
    agent any

    parameters {
        choice(
            name: 'ACTION',
            choices: ['apply', 'destroy'],
            description: 'Choose Terraform action'
        )
    }

    environment {
        AWS_REGION = "ap-southeast-1"
        AWS_ACCESS_KEY_ID     = credentials('aws-access-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/itsdevopsprofile/terraform-pipeline.git'
            }
        }

        stage('Terraform Init') {
            steps {
                sh 'terraform init'
            }
        }

        stage('Terraform Plan') {
            steps {
                sh 'terraform plan -out=tfplan'
            }
        }

        stage('Terraform Apply') {
            when {
                expression { params.ACTION == 'apply' }
            }
            steps {
                sh 'terraform apply -auto-approve tfplan'
            }
        }

        stage('Terraform Destroy') {
            when {
                expression { params.ACTION == 'destroy' }
            }
            steps {
                sh 'terraform destroy -auto-approve'
            }
        }
    }
}

````

### Apply 
