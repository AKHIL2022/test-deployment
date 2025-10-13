pipeline {
    agent any

    stages {
        stage('Install') {
            steps {
                script {
                    echo 'Running dummy install step...'
                    // Simulate npm install or any dependency installation
                    sh '''
                        echo "Installing dependencies..."
                        echo "npm install --save-dev some-package" > install.log
                        sleep 2
                        echo "Dependencies installed successfully."
                    '''
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    echo 'Running dummy build step...'
                    // Simulate a build process
                    sh '''
                        echo "Building project..."
                        echo "npm run build" > build.log
                        sleep 2
                        echo "Build completed successfully."
                    '''
                }
            }
        }
    }

    post {
        always {
            script {
                echo 'Triggering e2e tests'
                def downstreamBuild = build job: 'testing/childJob', wait: true, propagate: false
                
                def downstreamStatus = downstreamBuild.result
                def downstreamBuildNumber = downstreamBuild.number
                
                echo "Downstream job 'DownstreamJob' #${downstreamBuildNumber} completed with status: ${downstreamStatus}"

                if (downstreamStatus != 'SUCCESS') {
                    unstable "Automation job 'testing/childJob' #${downstreamBuildNumber} failed with status: ${downstreamStatus}"
                }
            }
        }
    }
}
