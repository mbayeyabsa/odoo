

pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/mbayeyabsa/odoo.git', branch:'master'
      }
    }
    
      stage("Build image") {
            steps {
                script {
                    myapp = docker.build("odoo:${env.BUILD_ID}")
                }
                script {
                    myapp1 = docker.build("postgres:9.4}")
                }
            }
        }
    
      stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                            myapp1.push
                            myapp1.push("${env.BUILD_ID}")
                    }
                }
            }
        }

    
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "odoo.yaml", kubeconfigId: "mykubeconfig")
        }
      }
    }

  }

}
