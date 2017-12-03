node {
    def app

    stage('clone') {
        checkout scm
    }

    stage('build') {
        app = docker.build("josoder/devops_docker")
    }

    stage('test'){
        /* code runs inside the container */
        app.inside {
            sh 'node --version'
        }
    }

    stage('push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                    app.push("${env.BUILD_NUMBER}")
                                app.push("latest")
                                }
    }
    
     sshagent (credentials: ['docker-manager']) {
        stage('publish'){
            sh 'ssh -o StrictHostKeyChecking=no -l docker 52.29.132.137 docker service create --name react-app --publish 5000:5000/tcp josoder/devops_docker'
        }
    }
}
