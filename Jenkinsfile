pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Maven Build') {
            steps { sh 'mvn clean compile' }
        }
        stage('PMD Code Check') {
            steps { sh 'mvn pmd:pmd' }
            post { always { publishPMD pattern: '**/target/pmd.xml' } }
        }
        stage('Run Tests') {
            steps { sh 'mvn test' }
            post { always { junit '**/target/surefire-reports/*.xml' } }
        }
        stage('Package') {
            steps { sh 'mvn package -DskipTests' }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: '**/target/*.jar,**/target/*.war', allowEmptyArchive: true
        }
    }
}
