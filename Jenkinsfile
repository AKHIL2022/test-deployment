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
                echo 'Triggering downstream job: DownstreamJob'
                // Trigger the downstream job and wait for its completion
                def downstreamBuild = build job: 'DownstreamJob', wait: true, propagate: false

                // Fetch the downstream job's status
                def downstreamStatus = downstreamBuild.result
                def downstreamBuildNumber = downstreamBuild.number

                // Display the status in the parent job
                echo "Downstream job 'DownstreamJob' #${downstreamBuildNumber} completed with status: ${downstreamStatus}"

                // Optional: Fail the parent job if the downstream job failed
                if (downstreamStatus != 'SUCCESS') {
                    error "Downstream job 'DownstreamJob' #${downstreamBuildNumber} failed with status: ${downstreamStatus}"
                }
            }
        }
    }
}
