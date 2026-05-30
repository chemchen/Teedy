pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Maven Build & Install') {
            steps {
                sh 'mvn clean install -DskipTests'
            }
        }
        stage('PMD Code Check') {
            steps {
                sh 'mvn pmd:pmd'
            }
        }
        stage('Run Unit Tests') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Generate Test Report') {
            steps {
                sh 'mvn surefire-report:report'
            }
        }
        stage('Generate JavaDoc') {
            steps {
                sh 'mvn javadoc:javadoc -Ddoclint=none'
                sh 'mvn javadoc:jar -Ddoclint=none'
            }
        }
        stage('Package') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: '**/target/*.jar,**/target/*.war,**/target/*-javadoc.jar,**/target/site/surefire-report.html,**/target/pmd.xml', allowEmptyArchive: true
        }
    }
}
