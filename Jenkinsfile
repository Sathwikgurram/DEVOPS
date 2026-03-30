pipeline {
    agent any

    tools {
        nodejs 'node25'
    }

    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'node -v'
                        sh 'npm -v'
                        sh 'npm ci'
                    } else {
                        bat 'node -v'
                        bat 'npm -v'
                        bat 'npm ci'
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'npm test'
                    } else {
                        bat 'npm.cmd test'
                    }
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'npm run build'
                    } else {
                        bat 'npm.cmd run build'
                    }
                }
            }
        }

        stage('Docker Build') {
            when {
                expression {
                    return fileExists('DockerFile') || fileExists('Dockerfile')
                }
            }
            steps {
                script {
                    if (isUnix()) {
                        if (fileExists('DockerFile')) {
                            sh 'docker build -f DockerFile -t my-website:latest .'
                        } else {
                            sh 'docker build -t my-website:latest .'
                        }
                    } else {
                        if (fileExists('DockerFile')) {
                            bat 'docker build -f DockerFile -t my-website:latest .'
                        } else {
                            bat 'docker build -t my-website:latest .'
                        }
                    }
                }
            }
        }
    }
}
