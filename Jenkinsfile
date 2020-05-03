pipeline {
    agent any
    environment {
        DOCKER_HOST = 'tcp://127.0.0.1:2375'
    }
    parameters {
            string(description: 'Jenkins Build number', name: 'jenkinsBuildNum', defaultValue: env.BUILD_NUMBER)
        }

    stages {
        stage('Check out code') {
            steps {
                git 'https://github.com/shashank-ssriva/Employee-App.git'
            }
        }
        stage('SonarQube analysis') {
            steps {
                sh "/Users/admin/Downloads/sonar-scanner-4.0.0.1744-macosx/bin/sonar-scanner"
            }
        }
        stage('Build WAR + Create Docker image') {
            steps {
                sh '/usr/local/bin/mvn clean package'
            }
        }
        stage('Push Docker image to Nexus') {
            steps {
                sh '/usr/local/bin/mvn dockerfile:push'
            }
        }
        stage('Deploy on K8s') {
            steps {
                echo env.BUILD_NUMBER
                sh '/usr/local/bin/ansible-playbook ansible-k8s-deploymeny.yaml'
            }
        }    
    }
}