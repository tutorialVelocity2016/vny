def gitCommit() {
    sh "git rev-parse HEAD > GIT_COMMIT"
    def gitCommit = readFile('GIT_COMMIT').trim()
    sh "rm -f GIT_COMMIT"
    return gitCommit
}

node {
    // Checkout source code from Git
    stage 'Checkout'
    checkout scm

    // Build Docker image
    stage 'Build'
    sh "docker build -t mesosphere/vny:${gitCommit()} ."

    // Log in and push image to GitLab
    stage 'Publish'
    withCredentials(
        [[
            $class: 'UsernamePasswordMultiBinding',
            credentialsId: 'wbvny',
            passwordVariable: 'DOCKERHUB_PASSWORD',
            usernameVariable: 'DOCKERHUB_USERNAME'
        ]]
    ) {
        sh "docker login -u tutorialVelocity2015 -p drip4:bruise"
        sh "docker push mesosphere/vny:${gitCommit()}"
    }
}
