pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Maven Install') {
            steps {
                sh 'mvn clean install -DskipTests'
            }
        }
        stage('PMD Code Check') {
            steps {
                sh 'mvn pmd:pmd'
            }
            // 不额外发布，让 archiveArtifacts 归档 pmd.xml 即可
        }
        stage('Run Unit Tests') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }
        stage('Generate Test Report') {
            steps {
                sh 'mvn surefire-report:report'
            }
            post {
                always {
                    publishHTML([
                        reportDir: 'docs-web/target/site',
                        reportFiles: 'surefire-report.html',
                        reportName: 'Surefire Report',
                        allowMissing: true,
                        keepAll: true,
                        alwaysLinkToLastBuild: true
                    ])
                }
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
            archiveArtifacts artifacts: '**/target/*.jar,**/target/*.war,**/target/*-javadoc.jar,**/target/pmd.xml', allowEmptyArchive: true
        }
    }
}
