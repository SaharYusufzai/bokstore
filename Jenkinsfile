pipeline {
    agent any
    parameters {
        choice(name: 'DEPLOY_ENV', choices: ['dev', 'qat', 'stage', 'prod'], description: 'Choose the environment to deploy')
    }
    stages {
        stage('Building the app using maven') {
            steps {
                script {
                    sh "mvn clean install -P${params.DEPLOY_ENV}"
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh "mvn sonar:sonar -Dsonar.host.url=http://52.87.195.31:9000 -Dsonar.login=803f9ed646e53aa0264ac049ca1fa00773001570"
                }
            }
        }
        stage('Code Coverage') {
            steps {
                sh '''
                echo Running code coverage
                mvn jacoco:report
                '''
            }
        }
        stage('Deploy') {
            steps {
                echo "Deploying to ${params.DEPLOY_ENV}"
                echo "Deployment to ${params.DEPLOY_ENV} was successful"
            }
        }
    }
    post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'This will run only if the build is successful'
        }
        failure {
            echo 'This will run only if the build fails'
        }
    }
}
