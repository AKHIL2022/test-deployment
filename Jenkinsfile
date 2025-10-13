pipeline {
    agent any

    stages {
        stage('Install') {
            steps {
                script {
                    echo 'Checking for HTML file...'
                    sh '''
                        echo "Verifying test.html..."
                        if [ -f test.html ]; then
                            echo "Found test.html"
                            cat test.html > install_output.txt
                        else
                            echo "test.html not found"
                            exit 1
                        fi
                    '''
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    echo 'Processing HTML file...'
                    sh '''
                        echo "Building from test.html..."
                        cp test.html build_output.html
                        echo "<!-- Built by Jenkins -->" >> build_output.html
                        sleep 1
                        echo "Build completed."
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
