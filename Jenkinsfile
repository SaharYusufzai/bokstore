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
                    sh "mvn sonar:sonar -Dsonar.host.url=http://3.94.126.19:9000 -Dsonar.login=sqa_1cbc22df229a08dc22f548ec96c17802ff21ea2b"
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
