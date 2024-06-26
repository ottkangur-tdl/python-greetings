pipeline {
    agent any

    stages {
        stage('build-docker-image') {
            steps {
                build_docker_image()
            }
        }
        stage('deploy-to-dev') {
            steps {
                deploy('dev')
            }
        }
        stage('tests-on-dev') {
            steps {
                run_tests('dev')
            }
        }
        stage('deploy-to-stg') {
            steps {
                deploy('stg')
            }
        }
        stage('tests-on-stg') {
            steps {
                run_tests('stg')
            }
        }
        stage('deploy-to-prd') {
            steps {
                deploy('prd')
            }
        }
        stage('tests-on-prd') {
            steps {
                run_tests('prod')
            }
        }
    }
}

def build_docker_image(){
    echo "Build python-greetings and building docker image"
    sh 'docker build --no-cache -t ottkangur/python-greetings-app:latest .'
    sh 'docker push ottkangur/python-greetings-app:latest'
}

def deploy(String env){
    echo "Deploying to ${env} environment"
    sh "docker pull ottkangur/python-greetings-app:latest"
    sh "docker-compose stop python-greetings-app-${env}"
    sh "docker-compose down python-greetings-app-${env}"
    sh "docker-compose up -d python-greetings-app-${env}"
}

def run_tests(String env){
    echo "Running tests in ${env} environment"
    sh 'docker pull ottkangur/api-tests:latest'
    sh "docker run --network=host --rm ottkangur/api-tests:latest run greetings greetings_${env}"
}
