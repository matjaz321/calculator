pipeline {
    agent any
    stages {
        stage("Compile") {
            steps {
                sh "./gradlew compileJava"
            }
        }
        stage("Unit test") {
            steps {
                sh "./gradlew test"
            }
        }
        stage('Code coverage') {
            steps {
                sh "./gradlew test jacocoTestReport"
                publishHTML([
                    allowMissing: false,
                    alwaysLinkToLastBuild: false,
                    keepAll: false,
                    reportDir: 'build/reports/jacoco/test/html',
                    reportFiles: 'index.html',
                    reportName: 'JaCoCo Report',
                    reportTitles: 'JaCoCo Report'
                ])
                sh "./gradlew test jacocoTestCoverageVerification"
            }
        }
        stage('Static code analysis') {
            steps {
                sh "./gradlew checkstyleMain"
                publishHTML(target: [
                    reportDir: 'build/reports/checkstyle/',
                    reportFiles: 'main.html',
                    reportName: 'Checkstyle Report',
                ])
            }
        }
        stage('Package') {
            steps {
                sh "./gradlew build"
            }
        }
        stage('Docker build') {
            steps {
                sh "docker build -t matjaz321/calculator ."
            }
        }
        stage('Docker push') {
            steps {
                sh "docker push matjaz321/calculator"
            }
        }
    }
}