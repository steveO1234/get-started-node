node {
    def app

    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {
        app = docker.build("rvennam/getstartednode")
    }

    stage('Test image') {
        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
    stage('Deploy') {
        sh """
            template=`cat "deployment.yaml" | sed "s/<REGISTRY>/$MYREGISTRY/g" | sed "s/<NAMESPACE>/$MYNAMESPACE/g"`
            echo "$template" | kubectl apply -f -n default -'
            """
    }
}
