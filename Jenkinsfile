node {
    def app

    stage('Clone repository') {
      

        checkout scm
    }

    stage('Build image') {
  
       app = docker.build("maksat2416/testgitops")
    }

    stage('Test image') {
  

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        withCredentials([usernamePassword( credentialsId: 'docker-hub-credentials', usernameVariable: 'USER', passwordVariable: 'PASSWORD')]) {
            def registry_url = "registry.hub.docker.com/"
            bat "docker login -u $USER -p $PASSWORD ${registry_url}"
            docker.withRegistry("http://${registry_url}", "docker-hub-credentials") {
                // Push your image now
                bat "docker push username/foldername:build"
            }
        }
    }
    
    stage('Trigger ManifestUpdate') {
                echo "triggering updatemanifestjob"
                build job: 'updatemanifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
        }
}
