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
                   sh 'composer install'
            }
        }
        stage('Test') {
            steps {
                echo 'run unit test'
            }
        }
        stage('Deploy to UAT') {
            steps {
                echo 'Deploying to uat....'
                sh 'git remote add heroku1 git@heroku.com:neem-testapp.git'
                sh 'git push heroku1 origin/master:master' 
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