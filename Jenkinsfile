pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'Hema', url: 'https://github.com/papannah/maven-project.git']])
            }
        }
                stage('RunTest Cases') {
            steps {
                sh 'mvn clean test'
            }
        }
        stage('Publich Test Reports') {
            steps {
                sh 'ls /var/lib/jenkins/workspace/New_Demo_Pipeline/server/target/surefire-reports/'
                jacoco()
            }
        }
        stage('Build Code') {
            steps {
                sh 'mvn package -DskipTests=true'
            }
        }
        stage('Archive Results') {
            steps {
                archiveArtifacts 'webapp/target/*.war'
            }
        }
        stage('Deploy Application') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'Tomcat_Deployer', path: '', url: 'http://44.203.156.188:8090/')], contextPath: 'FirstDemo', war: '**/target/*.war'
            }
        }
    }
}
