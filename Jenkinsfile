node('php'){
    stage('Clean'){
        deleteDir()
        sh 'ls -la'
    }
    
    stage('Fetch') {
        checkout scm
    }
    
   stage('build app'){
   sh 'composer install --no-scripts --prefer-dist --no-dev --ignore-platform-reqs'
 }
    
    stage('Build'){
        sh 'composer install --no-scripts --prefer-dist --no-dev --ignore-platform-reqs'
    }
    
    stage('config') {
        parallel(
            'config cache': {
                echo 'Tarefa paralela 01' 
            },
            'config route': {
                echo 'Tarefa Paralela 02'
            }
        )
    }
    stage('Docker Build') {
        sh 'docker build -t mauromantovani/laravel:$BRANCH_NAME-$BUILD_NUMBER .'
    }
    
    stage('Docker Ship MM') {
        sh 'docker push mauromantovani/laravel:$BUILD_NUMBER'
        sh 'docker rmi -f mauromantovani/laravel:$BUILD_NUMBER' // deleta as imagens antigas 
    }
}
