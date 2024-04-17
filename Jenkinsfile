pipeline {
    agent any
    parameters {
        choice(name: 'DEPLOY_ENV', choices: ['dev', 'qat', 'stage', 'prod'], description: 'Choose the environment to deploy')
    }
    stages {
        stage('Building the app using maven') {
            steps {
                script {
                    if (params.DEPLOY_ENV == 'dev') {
                        sh 'mvn clean install -Pdev'
                    } else if (params.DEPLOY_ENV == 'qat') {
                        sh 'mvn clean install -Pqat'
                    } else if (params.DEPLOY_ENV == 'stage') {
                        sh 'mvn clean install -Pstage'
                    } else if (params.DEPLOY_ENV == 'prod') {
                        sh 'mvn clean install -Pprod'
                    } else {
                        echo 'No valid environment selected for deployment'
                    }
                }
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
