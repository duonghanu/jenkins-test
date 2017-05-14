pipeline {
    
    agent none
    
    stages {

        stage('Unit Tests') {
            agent {
                label 'apache'
            }

            steps {
                sh 'ant -f test.xml -v'
                junit 'reports/result.xml'
            }
        }

        stage('build') {
            agent {
                label 'apache'
            }

            steps {
                sh 'ant -f build.xml -v'
            }

            post {
                success {
                   archiveArtifacts artifacts: 'dist/*.jar', fingerprint:true
                }
            }            
        }

        stage('deploy') {
            agent {
                label 'apache'
            }

            steps {
                sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/"
            }
        }

        /*
        stage('Running on Ubuntu') {
            agent {
                label 'Ubuntu'
            }

            steps {
                sh "wget http://ec2-34-204-3-119.compute-1.amazonaws.com/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar"
                sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4"
            }
        }
        */

        stage('Running on Debian') {
            agent {
                docker "openjdk:8u121-jre"
            }

            steps {
                sh "wget http://ec2-34-204-3-119.compute-1.amazonaws.com/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar"
                sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4"                
            }
        }

        state('Promote to Green') {
            steps {
                sh "cp /var/www/html/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/green/rectangle_${env.BUILD_NUMBER}.jar"
            }
        }
    }

}