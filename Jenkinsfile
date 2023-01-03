pipeline {
    agent {label 'DEV'}

    stages {
        stage('clone') {
            steps {
                dir('backend'){
                   git branch: 'master', url: 'https://github.com/jhossmar/MusicShopDjango.git'
                   
                }
                dir('frontend'){
                    git branch: 'main', url: 'https://github.com/jhossmar/proyecto-final-vue.git'
                    
                }
            }
        }
        stage ('docker build'){
            steps{
               dir('backend'){
                sh "docker build -t musicshopdjango:1.0 ."

               }
               dir('frontend'){
                sh "docker build -t proyecto_vue/final:v1  ."
               }
            }
        }
        stage('Deploy  DEV'){
            steps{
                sh "mkdir -p /home/marcelo/Final"
                dir ('/home/marcelo/Final'){
                   cleanWs()
                   git branch: 'main', url: 'https://github.com/jhossmar/jenkinsfiles.git' 
                   sh "docker-compose rm -sf"
                   sh "docker-compose up -d"
                }
            }
        }
    }
}
