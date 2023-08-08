pipeline {
    agent any
    triggers {
        githubPush()
    }
    options {
        timestamps()
    }
    stages {
        stage('Github Clone & Checkout'){
            steps {
                git branch: 'main', 
                    url: 'https://github.com/dandaeki/jenkins-cicd.git',
                    credentialsId: 'github-jenkins-cicd'
            }
        }
        stage('httpd-dockerfile-Build') {
            steps {
                script {
                    // docker hub에 로그인 하기 위해 사용용
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub') {
                        //  Dockerfile 로 이미지 생성
                        docker.build('dandaeki/jenkins-httpd', './mission/ubuntu_apache2')
                    }
                    
                }
            }
        }
        stage('nginx-image-Build') {
            steps {
                script {
                    // docker hub에 로그인 하기 위해 사용용
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub') {
                        //  Dockerfile 로 이미지 생성
                        docker.build('dandaeki/jenkins-nginx', './mission/ubuntu_nginx')
                    }
                    
                }
            }
        }
        stage('nginxLB-Build') {
            steps {
                script {
                    // docker hub에 로그인 하기 위해 사용용
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub') {
                        //  Dockerfile 로 이미지 생성
                        docker.build('dandaeki/jenkins-nginxloadbalancer', './mission/ubuntu_nginx_loadbalancer')
                    }
                    
                }
            }
        }
        stage('httpd Image Push') {
            steps {
                script {
                    // docker hub에 로그인 하기 위해 사용용
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub') {
                        def img = docker.image('dandaeki/jenkins-httpd')
                        img.push('0.1')
                        img.push('latest')
                    }
                    
                }
            }
        }
        stage('nginx Image Push') {
            steps {
                script {
                    // docker hub에 로그인 하기 위해 사용용
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub') {
                        def img = docker.image('dandaeki/jenkins-nginx')
                        img.push('0.1')
                        img.push('latest')
                    }
                    
                }
            }
        }
        stage('nginxLB Image Push') {
            steps {
                script {
                    // docker hub에 로그인 하기 위해 사용용
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub') {
                        def img = docker.image('dandaeki/jenkins-nginxloadbalancer')
                        img.push('0.1')
                        img.push('latest')
                    }
                    
                }
            }
        }
    }
    post {
        cleanup {
            emailext subject: '$DEFAULT_SUBJECT',
                    to: 'zzwxc2006@gmail.com',
                    body: '$DEFAULT_CONTENT'
            cleanWs() // 작업영역 삭제
        }
    }
}

