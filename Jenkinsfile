def podtemplate='''
apiVersion: v1
kind: Pod
metadata:
  labels:
    type: k8-tools
spec:
  serviceAccountName: k8-tools-sa
  containers:
  - image: ikambarov/k8-tools
    name: k8-tools
    args:
    - sleep
    - "100000"
'''

 
if(env.BRANCH_NAME ==~ "dev-.*"){
    environment="dev"
    region="us-east-1"
}
else if(env.BRANCH_NAME ==~ "qa-.*"){
    environment="qa"
    region="us-east-2"
}
else{
    environment="prod"
    region="us-west-2"
}


podTemplate(label: 'k8-tools', name: 'k8-tools', namespace: 'tools', yaml: podtemplate, showRawYaml: false) {
  node("k8-tools"){
    container("k8-tools"){
        stage("Pull"){
          checkout scm
        }

        withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'REGISTRY_PASSWORD', usernameVariable: 'REGISTRY_USERNAME')]) {
          stage("Build"){
            sh 'docker build -t $REGISTRY_USERNAME/flaskex .'
          }

          stage("Push Image"){
            sh '''
              docker login -u $REGISTRY_USERNAME -p $REGISTRY_PASSWORD
              docker push $REGISTRY_USERNAME/flaskex:$
            '''
          }
        }    

        stage("deploy"){
          sh '''
            
          '''
        }
    }
  }
}
