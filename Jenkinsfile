pipeline {
    agent any
    parameters {
        choice(name: 'DEPLOY_ENV', choices: ['dev', 'qat', 'stage', 'prod'], description: 'Choose the environment to deploy')
    }
    stages {
        stage('Deliver') {
            steps {
                sh 'mvn -B release:prepare release:perform'
                stash includes: '**/target/*.war', name: 'app'
            }
        }
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
        stage('Deploy to Dev Env') {
            when {
                expression { params.DEPLOY_ENV == 'dev' }
            }
            steps {
                sh './deploy-to-dev.sh'
            }
        }
        stage('Deploy to QAT Env') {
            when {
                expression { params.DEPLOY_ENV == 'qat' }
            }
            steps {
                echo 'Deploying to QAT Environment'
                unstash 'app'
            }
        }
        stage('Deploy to Stage Env') {
            when {
                expression { params.DEPLOY_ENV == 'stage' }
            }
            steps {
                sh './deploy-to-stage.sh'
            }
        }
        stage('Deploy to Prod Env') {
            when {
                expression { params.DEPLOY_ENV == 'prod' }
            }
            steps {
                sh './deploy-to-prod.sh'
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
