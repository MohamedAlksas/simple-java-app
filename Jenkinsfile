node {
    git branch: 'main', url: 'https://github.com/DinaGamalMahmoud/simple-java-app.git'
    stage('Build') {
        sh 'echo "Building the application..."'
    }
    stage('Test') {
        if (env.BRANCH_NAME == 'feat') {
            sh 'echo "Running tests..."'
        }
        else {
            sh 'echo "Skipping tests for non-feat branches."'
        }
    }
}
