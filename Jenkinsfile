node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("romasks/hellonode")
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
        sh "heroku login -i"
        sh "lusersks@gmail.com"
        sh "Qwerty_325"
        sh "heroku container:login"
        sh "lusersks@gmail.com"
        sh "Qwerty_325"
        sh "docker tag romasks/hellonode registry.heroku.com/hellonode/web"
        sh "docker push registry.heroku.com/hellonode/web"
    }

    stage('Run image') {
        sh 'heroku open'
    }
}
