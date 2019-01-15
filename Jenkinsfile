import groovy.json.JsonSlurper

pipeline {
    agent any
    environment {
        REDIS_HOST='localhost'
        DB_CONNECTION='pgsql'
        DB_HOST='ec2-13-126-138-166.ap-south-1.compute.amazonaws.com'
        DB_PORT='5434'
        DB_DATABASE='test'
        DB_USERNAME='postgres'
        DB_PASSWORD='postgres'
        REPO_URL='neemchand/jenkins-app'
        ACCESS_TOKEN= credentials('jenkins_app_git_token')
        PROJECT_NAME='openmind-api'

    }
    stages {
        stage('install database') {
             steps {
                 updateGithubStatus('pending')
                 sh 'docker-compose -f docker-compose.yml up -d pgsql'
                 sh 'docker-compose -f docker-compose.yml up -d pgadmin'
             }
        }
        stage('Unit Testing') {
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
    }
    post {
        success {
             script {
                            //I want to get the same response here
                            def response = sh(script: 'curl https://production-review-tool.herokuapp.com/api/checkReadyToDeploy?app_name='+env.PROJECT_NAME, returnStdout: true)
                            def json = new JsonSlurper().parseText(response)
                            def rejected_count = "${json.rejected_count}"
                            echo 'rejected count'+rejected_count
                            if (rejected_count == '0') {
                             
                            }

                        }

             updateGithubStatus('success')

        }
        failure {
             updateGithubStatus('failure')

        }
    }
}

void updateGithubStatus(status) {
     sh 'curl https://api.github.com/repos/narayan-ucreate/jenkins/statuses/'+env.git_COMMIT+'?access_token='+env.ACCESS_TOKEN+' --header "Content-Type: application/json" --data "{\\"state\\": \\"'+status+'\\", \\"description\\": \\"Jenkins\\"}"'
}

void checkReadyForDeploy(project_name) {
    sh(script: 'curl https://some-host/some-service/getApi?apikey=someKey', returnStdout: true)

    sh """
                    brf=${env.REDIS_HOST}
                    echo \$brf
                """



}

void notifyToSlack(status)
{
    sh 'curl https://production-review-tool.herokuapp.com/api/buildNotification --header "Content-Type: application/json" --request POST --data "{\\"payload\\" : {\\"build_parameters\\": {\\"CIRCLE_JOB\\" : \\"uat-push\\"}, \\"build_url\\" : \\"asdf\\", \\"committer_name\\" : \\"narayan\\", \\"status\\" : \\"'+status+'\\", \\"subject\\" : \\"Comment name\\", \\"reponame\\" : \\"ucreate-review-tool\\", \\"outcome\\" :\\"success\\",\\"branch\\" : \\"master\\"}}"'
}


