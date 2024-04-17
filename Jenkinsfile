pipeline {
    agent any
    stages {
        stage('Building the app using maven') {
            steps {
                sh '''
                echo Building the Maven application
                mvn clean install
                '''
            }
        }
        stage('SonarQube Analysis') {
            environment {
                scannerHome = tool 'SonarQubeScanner'
            }
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh "${scannerHome}/bin/sonar-scanner"
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
