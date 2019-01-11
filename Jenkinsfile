pipeline {
    agent {  docker { image 'ucreateit/php7.2:v0.1'}
                } 
    environment {
        APP_VERSION = '1'
        HEROKU_LOGIN = 'jagdeepsingh@ucreate.co.in'
        HEROKU_API_KEY ='b90f6c35-7b07-4c13-a262-26cee3b241e0'
    }
    stages {
        stage('Build') {
            steps {
                   sh 'php --version'
            }
        }
        stage('Test') {
            steps {
                echo 'run unit test'
            }
        }
        stage('Deploy to UAT') {
            steps {
                echo 'Deploying to uat..32w' 
                sh '''git remote rm heroku
                git remote add heroku https://git.heroku.com/neem-jenkins-app.git
                
                wget https://cli-assets.heroku.com/branches/stable/heroku-linux-amd64.tar.gz
                sudo mkdir -p /usr/local/lib /usr/local/bin
                   sudo tar -xvzf heroku-linux-amd64.tar.gz -C /usr/local/lib
                    sudo ln -s /usr/local/lib/heroku/bin/heroku /usr/local/bin/heroku

                     cat > ~/.netrc << EOF
                    machine api.heroku.com
                      login jagdeepsingh@ucreate.co.in
                      password b90f6c35-7b07-4c13-a262-26cee3b241e0
                    machine git.heroku.com
                      login jagdeepsingh@ucreate.co.in
                      password b90f6c35-7b07-4c13-a262-26cee3b241e0
                    EOF

                    # Add heroku.com to the list of known hosts
                    ssh-keyscan -H heroku.com >> ~/.ssh/known_hosts'''

               
            }
        }
    }
    post {
        always {
          echo 'test Done...1.'
      }

      success {
          echo 'test ...2.'
      }

      failure {
          echo 'failure'
      }
    }
}