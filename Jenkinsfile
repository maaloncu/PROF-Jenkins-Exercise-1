pipeline {
    agent any
    
    triggers {
        githubPush()
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo "Building from: ${GIT_BRANCH}"
                checkout scm
            }
        }
        
        stage('Build and Test') {
            steps {
                echo 'Running Maven verify with JaCoCo analysis'
                sh 'mvn -B -DskipTests=false clean verify'
            }
        }
    }
    
    post {
        always {
            echo 'Archiving JaCoCo coverage report'
            archiveArtifacts artifacts: 'target/site/jacoco/**', allowEmptyArchive: true
            publishHTML([
                reportDir: 'target/site/jacoco',
                reportFiles: 'index.html',
                reportName: 'JaCoCo Coverage Report',
                keepAll: true
            ])
        }
        
        failure {
            echo 'Build failed - Check coverage requirements'
        }
        
        success {
            echo 'Build completed successfully'
        }
    }
}
