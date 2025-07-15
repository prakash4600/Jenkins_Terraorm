pipeline {
  agent any

  environment {
    ARM_CLIENT_ID       = credentials('AZURE_CLIENT_ID')       // Jenkins Credentials
    ARM_CLIENT_SECRET   = credentials('AZURE_CLIENT_SECRET')
    ARM_SUBSCRIPTION_ID = credentials('AZURE_SUBSCRIPTION_ID')
    ARM_TENANT_ID       = credentials('AZURE_TENANT_ID')
  }

  stages {
  stage('Checkout Code') {
    steps {
      git branch: 'main', url: 'https://github.com/prakash4600/Jenkins_Terraorm.git'
    }  
  }

    stage('Terraform Init') {
      steps {
        dir('terraform') {
          sh 'terraform init'
        }
      }
    }

    stage('Terraform Plan') {
      steps {
        dir('terraform') {
          sh '''
            terraform plan \
              -var="client_id=$ARM_CLIENT_ID" \
              -var="client_secret=$ARM_CLIENT_SECRET" \
              -var="subscription_id=$ARM_SUBSCRIPTION_ID" \
              -var="tenant_id=$ARM_TENANT_ID"
          '''
        }
      }
    }

    stage('Terraform Apply') {
      steps {
        dir('terraform') {
          sh '''
            terraform apply -auto-approve \
              -var="client_id=$ARM_CLIENT_ID" \
              -var="client_secret=$ARM_CLIENT_SECRET" \
              -var="subscription_id=$ARM_SUBSCRIPTION_ID" \
              -var="tenant_id=$ARM_TENANT_ID"
          '''
        }
      }
    }
  }

  post {
    failure {
      echo '❌ Deployment failed. Please check the logs above.'
    }
    success {
      echo '✅ Jenkins VM deployed successfully via Terraform!'
    }
  }
}
