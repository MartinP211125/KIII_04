node {
    def app
    stage('Clone repository') {
        checkout scm
    }
    
    // Check if branch is dev
    def isDevBranch = (env.BRANCH_NAME == 'dev')

    // Only proceed if changes are in the dev branch
    if (isDevBranch) {
        stage('Build image') {
            app = docker.build("martin211125/kiii_04")
        }
        stage('Push image') {
            docker.withRegistry('', 'dockerhub') {
                app.push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
                app.push("${env.BRANCH_NAME}-latest")
                // signal the orchestrator that there is a new version
            }
        }
    } else {
        echo 'Changes are not in the dev branch. Skipping build and push steps.'
    }
}
