def podtemplate = '''
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
properties([
  parameters([
    choice(choices: ['dev', 'qa','stage', 'prod'], description: 'Choose an ENV from the list', name: 'environment'),
    booleanParam(defaultValue: true, description: 'Do you want to apply?', name: 'command')
  ])
])

podTemplate(label: 'k8-tools', name: 'k8-tools', namespace: 'tools', yaml: podtemplate, showRawYaml: false) {
  node("k8-tools"){
    container("k8-tools"){

      
      stage("Pull"){
        git 'https://github.com/rahymov/flask-chart.git'
      }
      withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'REGISTRY_PASSWORD', usernameVariable: 'REGISTRY_USERNAME')]) {
        stage("deploy"){
          sh '''
            helm upgrade --install my --set image.repository=$REGISTRY_USERNAME/flaskex:$environment .  -n default
          '''
        }
      }
    }
  }
}

