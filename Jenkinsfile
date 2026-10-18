pipeline {
    agent any

    environment {
        APP_IMAGE = 'blog-sederhana:latest'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                echo 'Menarik kode dari GitHub...'
                checkout scm
            }
        }

        stage('Check Database Configuration') {
            steps {
                echo 'Mengecek ketersediaan konfigurasi database...'
                // Mengecek keberadaan file konfigurasi database Laravel
                script {
                    if (isUnix()) {
                        sh 'ls -la config/database.php'
                    } else {
                        bat 'dir config\\database.php'
                    }
                }
            }
        }

        stage('Docker Build Stage') {
            steps {
                echo 'Membangun Docker Image...'
                script {
                    if (isUnix()) {
                        sh "docker build -t ${APP_IMAGE} ."
                    } else {
                        bat "docker build -t ${APP_IMAGE} ."
                    }
                }
            }
        }

        stage('Docker Test Stage') {
            steps {
                echo 'Menjalankan container uji coba...'
                script {
                    if (isUnix()) {
                        sh "docker run -d --name blog-test-container ${APP_IMAGE} || true"
                        sh "docker ps"
                    } else {
                        bat "docker run -d --name blog-test-container %APP_IMAGE% || exit 0"
                        bat "docker ps"
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Membersihkan container uji coba...'
            script {
                if (isUnix()) {
                    sh 'docker stop blog-test-container || true'
                    sh 'docker rm blog-test-container || true'
                } else {
                    bat 'docker stop blog-test-container || exit 0'
                    bat 'docker rm blog-test-container || exit 0'
                }
            }
        }
        success {
            echo 'UYEY SUKSES!'
        }
    }
}
