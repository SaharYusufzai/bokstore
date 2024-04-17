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
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh "mvn sonar:sonar -Dsonar.host.url=http://3.94.126.19:9000 -Dsonar.login=d37e9392acd399dc4c0b9228f7a0631819f900e8"
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
