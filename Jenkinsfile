pipeline {
    agent {label 'DEV'}

    stages {
        stage('clone') {
            steps {
                dir('backend'){
                   git 'https://github.com/jhossmar/MusicShopDjango.git'

                }
                dir('frontend'){
                    git 'https://github.com/jhossmar/proyecto-final-vue.git'
                }
            }
        }
    }
}
