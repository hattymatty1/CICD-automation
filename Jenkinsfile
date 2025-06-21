pipeline {
  agent any

  environment {
    EC2_USER = "ubuntu"
    EC2_B_IP = "54.83.102.76" // replace with actual EC2-B IP
    CRED_ID = "target"
  }

  stages {
    stage('Checkout Code') {
      steps {
        git branch: 'main', url: 'https://github.com/hattymatty1/CICD-automation.git'
      }
    }

    stage('Install Apache and Deploy Page') {
      steps {
        sshagent (credentials: [CRED_ID]) {
          sh """
            ssh -o StrictHostKeyChecking=no $EC2_USER@$EC2_B_IP << 'EOF'
              sudo apt update
              sudo apt install apache2 -y
              sudo systemctl enable apache2
              sudo systemctl start apache2
            EOF

            scp -o StrictHostKeyChecking=no index.html $EC2_USER@$EC2_B_IP:/tmp/index.html

            ssh -o StrictHostKeyChecking=no $EC2_USER@$EC2_B_IP << 'EOF'
              sudo mv /tmp/index.html /var/www/html/index.html
              sudo chown www-data:www-data /var/www/html/index.html
            EOF
          """
        }
      }
    }
  }
}

