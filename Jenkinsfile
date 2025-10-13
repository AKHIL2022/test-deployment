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
stage('Trigger Child Job and Continue') {
    steps {
        script {
            def childJobResult = build job: 'testing/child-job',
                                      wait: false,
                                      propagate: false
            def childJobUrl = childJobResult.getAbsoluteUrl()
            echo "Child job triggered: ${childJobUrl}"
        }
    }
}
}
}
