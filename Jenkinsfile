pipeline {
    agent {
        label "agent2"
    }
    stages {
        stage('Clean up') {
            steps {
                deleteDir()
            }
        }
        stage('Clone GIT repo') {
            steps {
                sh 'git clone https://github.com/bkelava/kubernetes.git'
            }
        }
        stage ('Update kubernetes configuration file') {
            steps {
                dir('kubernetes') {
                    withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'github_password', usernameVariable: 'github_username')]) {
                        sh "git config user.name bkelava"
                        sh "git config user.email bozidar.kelava@gmail.com"
                        sh """#!/bin/bash
                            sed -i 's,\\(image: kelava/kub-demo-auth:\\).*,\\1${params.DOCKERTAG},g' kubernetes.yaml
                        """
                        sh """#!/bin/bash
                            sed -i 's,\\(image: kelava/kub-demo-frontend:\\).*,\\1${params.DOCKERTAG},g' kubernetes.yaml
                        """
                        sh """#!/bin/bash
                            sed -i 's,\\(image: kelava/kub-demo-tasks:\\).*,\\1${params.DOCKERTAG},g' kubernetes.yaml
                        """
                        sh """#!/bin/bash
                            sed -i 's,\\(image: kelava/kub-demo-users:\\).*,\\1${params.DOCKERTAG},g' kubernetes.yaml
                        """
                        sh "git add ."
                        sh """#!/bin/bash
                            git commit -m 'Done by Jenkins Job --Update kubernetes configuration: ${env.BUILD_NUMBER}'
                        """
                        sh """#!/bin/bash
                            git push https://${github_username}:${github_password}@github.com/${github_username}/kubernetes.git HEAD:main
                        """
                    }
                }
            }
        }
    }
}
