pipeline {
    agent any
    tools {
        go 'go-1.19' // Comes from the jenkins global config
    }
    environment {
        ENV = "${env.BRANCH_NAME == 'master' ? 'PROD' : 'DEV'}" // Define the ENV based on the branch name
    }

    stages {
        stage('Build') {
            steps {
                sh 'bash scripts/build.sh' // Run the build
            }
        }
        stage('Test') {
            steps {
                sh 'bash scripts/test.sh' // Run the test
            }
        }

        // NEW DEPLOY STAGE
        stage('Deploy') {
            when {

                /*
                anyOf ensures that this stage will only be triggered when the
                values mentioned match.
                With below config, the deploy stage ONLY triggers if the branch is
                develop or master.
                */
                anyOf {
                    branch 'master';
                    branch 'develop'
                }
            }
            steps {
                // Run the deployment script as mentioned in the task.
                sh 'export JENKINS_NODE_COOKIE=do_not_kill ; bash scripts/deploy.sh'
            }
        }
    }
}
