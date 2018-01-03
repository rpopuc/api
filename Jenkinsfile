node('php') {
    stage('Preparação'){
        deleteDir()
    }

    stage('Obtenção') {
        checkout scm
    }

    stage('Construção'){
        sh 'composer install --prefer-dist --ignore-platform-reqs'
    }

    stage('Configuração') {
        parallel(
            'config app': {
                sh 'cp .env.example .env'
                sh 'php artisan key:generate'
                sh 'php artisan config:cache'
            }
        )
    }
    stage('Testes') {
         sh './vendor/bin/phpunit'
    }
}

node('docker') {
    def app

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */
        app = docker.build("rpopuc/todoapi")
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("latest")
        }
    }
}