pipeline {
    agent {  docker { image 'ucreateit/php7.2:v0.1'}
                } 
    environment {
        APP_VERSION = '1'
        HEROKU_LOGIN = credentials('heroku_login')
        HEROKU_APP_NAME = credentials('heroku_app_name')
        HEROKU_API_KEY = credentials('heroku_jd_acc_api_key')
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
                echo 'Deploying to uat..' 
                sh '''git remote rm heroku
                git remote add heroku https://git.heroku.com/$HEROKU_APP_NAME.git
                git push --force https://heroku:$HEROKU_API_KEY@git.heroku.com/$HEROKU_APP_NAME.git HEAD:refs/heads/master
                '''
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