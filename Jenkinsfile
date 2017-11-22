podTemplate(label: 'mypod', containers: [
    containerTemplate(name: 'alpine', image: 'alpine:3.6', ttyEnabled: true, command: 'cat', args: '')
  ]) {

    node('mypod') {
        stage('Test a demo job') {
            git url: 'https://github.com/tAoD/first-blood.git'
            container('alpine') {
                stage('Build a Java project') {
                    sh """
                    cat LICENSE
                    """
                }
            }
        }
    }
}
