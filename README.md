# first-blood

Demo project that use with jenkins and kubernetes.

More information. Try github pull request builder.

## Setup jenkins master

### Install plugins

Depends on plugins:

- [Kubernetes plugin][https://wiki.jenkins-ci.org/display/JENKINS/Kubernetes+Plugin]
- [GitHub Pull Request Builder][https://wiki.jenkins-ci.org/display/JENKINS/GitHub+pull+request+builder+plugin]

Please check them at `Manage` --> `Plugin Manager`.

### Configure Kubernetes plugin

After Kubernetes plugin installed, go to `Manage Jenkins` -> `Configure System` -> `Cloud` section. 

- Add a `Kubernetes` cloud and an unique name like `my-k8s-cluster`.
- Leave `Kubernetes URL`/`Kubernetes server certificate key`/`Kubernetes Namespace` if jenkins in cluster.
- Set `Jenkins URL` to `http://<jenkins-service>:<jenkins-port>` if jenkins in cluster.
- Set `Container Cap` to a smaller number if k8s cluster is tiny, ep. `2`.

### Configure Github Pull Request Builder plugin

Go to `Manage Jenkins` -> `Configure System` -> `GitHub Pull Request Builder` section. 

Read [more][https://github.com/jenkinsci/ghprb-plugin] for what you need.

## Config kubernetes slave config

### Complete `Jenkinsfile`

Add `Jenkinsfile` to root of project, and config like following.

```
podTemplate(label: 'mypod', containers: [
    containerTemplate(name: 'alpine', image: 'alpine:3.6', ttyEnabled: true, command: 'cat')
  ]) {

    node('mypod') {
        stage('Test a demo job') {
            git url: 'https://github.com/tAoD/first-blood.git'
            container('alpine') {
                stage('Build a Java project') {
                    sh """
                    echo LICENSE
                    """
                }
            }
        }
    }
}
```

Go home, byebye.
