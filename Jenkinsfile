pipeline {
    agent {  docker { image 'ucreateit/php7.2:v0.1'}
                } 
    environment {
        APP_VERSION = '1'
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
            script{    
                if [ "" != "$(git remote |grep -e '^heroku$')" ]; then
                    git remote rm heroku
                fi

                git remote add heroku git@heroku.com:neem-jenkins-app.git
                git push -f heroku HEAD:master
               }
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