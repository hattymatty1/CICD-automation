pipeline {
  agent any

  environment {
    EC2_USER = "ubuntu"
    EC2_B_IP = "54.83.102.76"
    CRED_ID = "target" // Jenkins SSH credential ID
    REPO_URL = "https://github.com/hattymatty1/CICD-automation.git"
    APP_DIR = "/home/ubuntu/cicd-app"
  }

  stages {
    stage('Deploy index.html from GitHub') {
      steps {
        sshagent (credentials: [CRED_ID]) {
          sh """
            ssh -o StrictHostKeyChecking=no $EC2_USER@$EC2_B_IP '
              set -e

              # Install Apache and Git
              sudo apt update
              sudo apt install -y apache2 git

              # Clone or update the repo
              if [ ! -d "$APP_DIR" ]; then
                git clone $REPO_URL $APP_DIR
              else
                cd $APP_DIR && git pull
              fi

              # Replace default Apache index.html with your custom one
              sudo cp $APP_DIR/index.html /var/www/html/index.html
              sudo chown www-data:www-data /var/www/html/index.html

              # Restart Apache to ensure changes are served
              sudo systemctl restart apache2
            '
          """
        }
      }
    }
  }
}


