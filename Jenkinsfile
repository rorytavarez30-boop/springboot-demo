pipeline {
    agent any

    tools {
        // Maven installation named "maven3" in Manage Jenkins > Tools
        maven 'maven3'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Test') {
            steps {
                sh 'mvn -B test'
            }
        }
        stage('Build') {
            steps {
                // Builds the fat jar (app + all dependencies in one file)
                sh 'mvn -B clean package -DskipTests'
                sh 'ls -lh target/*.jar'
            }
        }
        stage('Verify Fat Jar') {
            steps {
                // Shows the dependency jars packed inside BOOT-INF/lib
                sh 'jar tf target/springboot-demo-0.0.1-SNAPSHOT.jar | grep BOOT-INF/lib | head -10'
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
    }
}
