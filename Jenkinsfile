pipeline {
    agent any
  
    environment {
        APP_VERSION = '1'
        HEROKU_LOGIN = credentials('heroku_login')
        HEROKU_APP_NAME = credentials('heroku_app_name')
        HEROKU_API_KEY = credentials('heroku_jd_acc_api_key')
        REDIS_HOST='localhost'
        DB_CONNECTION='pgsql'
        DB_HOST='ec2-13-126-138-166.ap-south-1.compute.amazonaws.com'
        DB_PORT='5434'
        DB_DATABASE='test'
        DB_USERNAME='postgres'
        DB_PASSWORD='postgres'

    }
    options {
    skipDefaultCheckout true
    }
    stages {
        stage('install database') {
               steps {
                //sh 'curl https://api.github.com/repos/neemchand/jenkins-app/statuses/9101f3d8fb3845de571ae65a63b740e785f112ef?access_token="d843bb6d8711c6355dfdb1202aa9a45ca2007ee5" --header "Content-Type: application/json" --data "{state: "success", description: "Jenkins"}"'

                sh 'docker-compose -f docker-compose.yml up -d pgsql'
                sh 'docker-compose -f docker-compose.yml up -d pgadmin'

               }
       }
       stage('install php') {
                   agent {
                       docker { image 'ucreateit/php7.2:v0.1' }
                   }

                steps {
                       sh "php -r \"copy('.env.example', '.env');\""
                       sh 'php artisan key:generate'
                       sh 'composer install -n --prefer-dist'
                       sh './vendor/bin/phpunit'
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
                sh '''
                git remote rm heroku
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