pipeline {
    agent any

    environment {
        NODE_ENV = 'production'
        SONARQUBE = 'sonarserver' // The name of your SonarQube instance in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    def nodejs = tool name: 'Node16', type: 'NodeJS'
                    env.PATH = "${nodejs}/bin:${env.PATH}"
                }
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'sonar-scanner -Dsonar.projectKey=my-nodejs-app -Dsonar.sources=. -Dsonar.language=js -Dsonar.node.source=node'
                }
            }
        }

        stage('Build App') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy App') {
            steps {
                sh 'npm run start'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/build/**', allowEmptyArchive: true
        }
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}
