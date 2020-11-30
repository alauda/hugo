library 'alauda-cicd'

pipeline {
    agent {
        label 'base'
    }
    // agent any
    stages {
        stage('checkout') {
            steps {
                checkout scm
            }
        }
        stage("login"){
            steps {
                script {
                    container('tools'){
                        deploy.dockerBuildWithRegister([:]).login()
                    }
                }
            }
        }
        stage('build') {
            steps {
                script {
                    container('tools') {
                        cmd = "docker buildx build -t harbor-b.alauda.cn/alauda/hugo:v0.59.1 --build-arg CGO=0 -f Dockerfile --platform linux/amd64,linux/arm64 --progress plain --push . "
                        sh script: cmd, label: cmd

                        // scancmd = "acp harbor scan --wait-report --url=harbor-b.alauda.cn --repository=base/golang --tag=1.15-alpine --username=**** --password=**** --gate=Critical --timeout=300 --insecure"
                        // deploy.dockerBuildWithRegister([:]).scan("harbor-b.alauda.cn/base/${parts.get(0)}", "${tag}")
                    }
                }
            }
        }
        stage('test') {
            // when {
            //     expression {
            //         BRANCH_NAME == "master"
            //     }
            // }
            steps {
                script {
                    dir("app") {
                        container('tools') {
                            // cmd = "docker buildx build -t app:test -f Dockerfile --platform linux/amd64,linux/arm64 --progress plain . "
                            // sh script: cmd, label: cmd
                        }
                    }
                }
            }
        }
    }
}