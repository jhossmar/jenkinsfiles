pipeline {
    agent {label 'DEV'}

    stages {
        stage('clone') {
            steps {
                dir('backend'){
                   git branch: 'master', 'https://github.com/jhossmar/MusicShopDjango.git'

                }
                dir('frontend'){
                    git branch: 'main', url: 'https://github.com/jhossmar/proyecto-final-vue.git'
                }
            }
        }
    }
}
