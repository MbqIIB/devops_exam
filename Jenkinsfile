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
    
    stage('publish') {
        sh 'docker run -d -i -p 5000:5000 josoder/devops_docker'
    }    
}
