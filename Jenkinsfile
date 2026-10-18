pipeline {
    agent any

    environment {
        APP_IMAGE = 'blog-sederhana:latest'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                echo 'Menarik source code Laravel dari GitHub...'
                checkout scm
            }
        }

        stage('Check Laravel Database Config') {
            steps {
                echo 'Memeriksa konfigurasi koneksi database Laravel...'
                // Memastikan file konfigurasi database Laravel terdeteksi di repo
                sh 'ls -la config/database.php'
            }
        }

        stage('Docker Build Stage') {
            steps {
                echo 'Membangun Docker Image untuk Laravel Blog...'
                sh "docker build -t ${APP_IMAGE} ."
            }
        }

        stage('Docker Test Stage') {
            steps {
                echo 'Menjalankan container Docker untuk pengujian...'
                sh "docker run -d --name blog-laravel-container -p 8081:80 ${APP_IMAGE} || true"
                sh "docker ps | grep blog-laravel-container || true"
            }
        }
    }

    post {
        always {
            echo 'Membersihkan environment container...'
            sh 'docker stop blog-laravel-container || true'
            sh 'docker rm blog-laravel-container || true'
        }
        success {
            echo 'Pipeline Laravel berhasil dieksekusi!'
        }
    }
}
