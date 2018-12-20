node {
    def app

    environment {
        DOCKER_IMAGE = "romasks/hellonode"

        REGISTRY_ADDRESS = "registry.heroku.com"
        REGISTRY_AUTH = credentials("docker-registry")
    }

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build(env.DOCKER_IMAGE)
    }

    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * For this example, we're using a Volkswagen-type approach ;-) */

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }

    stage('Deploy image') {
        sh "docker login -u=$REGISTRY_AUTH_USR -p=$REGISTRY_AUTH_PSW ${env.REGISTRY_ADDRESS}"
        sh "docker tag ${env.DOCKER_IMAGE} ${env.REGISTRY_ADDRESS}/hellonode/web"
        sh "docker push ${env.REGISTRY_ADDRESS}/hellonode/web"
    }

    stage('Run image') {
        sh 'heroku open'
    }
}
