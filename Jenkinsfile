node('php') {
    stage('Preparação')
    {
        deleteDir()
    }

    stage('Obtenção')
    {
        checkout scm
    }

    stage('Construção')
    {
        sh 'composer install --prefer-dist --ignore-platform-reqs'
    }

    stage('Configuração')
    {
        parallel(
            'config app': {
                sh 'cp .env.example .env'
                sh 'php artisan key:generate'
                sh 'php artisan config:cache'
            }
        )
    }

    stage('Testes')
    {
         sh './vendor/bin/phpunit'
    }
}

node('docker') {
    def app

    stage('Criação imagem')
    {
        app = docker.build("rpopuc/todoapi")
    }

    stage('Publicação da imagem')
    {
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("latest")
        }
    }
}