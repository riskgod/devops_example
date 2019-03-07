pipeline {
    agent { docker 'node:10.10' }

    environment {
        DISABLE_AUTH = 'true'
        DB_ENGINE    = 'postgres'
    }

    stages {
        stage('build') {
            steps {
                sh 'npm --version'
            }
        }
    }

    post {
        always {
            junit 'build/reports/results/build_report.xml'
        }
        success {
            slackSend channel: '#aspen-backend',
                    color: 'good',
                    message: "The pipeline ${currentBuild.fullDisplayName} completed successfully."
        }
        failure {
            mail to: 'riskgod@sina.com',
                subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
                body: "Something is wrong with ${env.BUILD_URL}"
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
}