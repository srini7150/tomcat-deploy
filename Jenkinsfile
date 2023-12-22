pipeline {
    agent any
    tools {
        maven '3.9.1'
    }
    environment {     
        SONARQUBE_URL = "https://plain-peas-wash.loca.lt/"
        SONARQUBE_CREDS = credentials('SONARQUBE_CREDS')
    }
    stages {
        stage ('build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage ("sonar") {
            steps {
                script {
                    sh """
                    sonar-scanner -Dsonar.branch.name=${BRANCH_NAME} \
                    -Dsonar.host.url=${SONARQUBE_URL} \
                    -Dsonar.login=${SONARQUBE_CREDS_USR} \
                    -Dsonar.password=${SONARQUBE_CREDS_PSW} \
                    -Dsonar.projectVersion="1.0.0" """
                }

            }
        }
        stage("Quality Gate") {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
