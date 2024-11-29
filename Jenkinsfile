pipeline {
    agent { docker { image 'maven:3-eclipse-temurin-17-alpine' } }

    stages {
        stage('Build') {
            steps {
                sh 'mvn -B clean compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn -B verify'
            }

            post {
                always {
                    junit '**/target/surefire-reports/TEST-*.xml'
                }
            }
        }

        stage('Publish') {
            when{
                branch 'main'
            }

            steps {
                sh 'mvn -B package -DskipTests'
            }

            post {
                success {
                    archiveArtifacts artifacts: '**/target/*.jar', followSymlinks: false
                }
            }
        }
    }
}