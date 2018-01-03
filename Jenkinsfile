node('php'){
    stage('Preparação'){
        deleteDir()
        sh 'ls -la'
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
