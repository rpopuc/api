node('php'){
    stage('Clean'){
        deleteDir()
        sh 'ls -la'
    }

    stage('Fetch') {
        checkout scm
    }

    stage('Build'){
        sh 'composer install --prefer-dist --no-dev --ignore-platform-reqs'
    }

    stage('config') {
        parallel(
            'config cache': {
                sh 'php artisan config:cache'
            },
            'config route': {
                sh 'php artisan route:cache'
            }
            'config app': {
                sh 'cp .env.example .env'
                sh 'php artisan key:generate'
            }
        )
    }    
    stage('Test') {
         sh 'vendor/bin/phpunit'
    }
}
