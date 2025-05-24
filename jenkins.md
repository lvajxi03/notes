# Jenkins useful information

## Marking Jenkins build as unsable with groovy postbuild plugin

```groovy
if(manager.logContains("Could not login to FTP server")) {
    manager.addWarningBadge("FTP Login Failure")
    manager.createSummary("warning.gif").appendText("<h1>Failed to login to remote FTP Server!</h1>", false, false, false, "red")
    manager.buildUnstable()
}
```

-- or --

```groovy
errpattern = ~/TEXT-TO-LOOK-FOR-IN-JENKINS-BUILD-OUTPUT.*/;

manager.build.logFile.eachLine{ line ->

    errmatcher=errpattern.matcher(line)

    if (errmatcher.find()) {

        manager.build.@result = hudson.model.Result.NEW-STATUS-TO-SET

    }

 }
 ```

Here:
* http://www.tikalk.com/devops/JenkinsJobStatusChange/

Links:

* Jenkins green balls: https://plugins.jenkins.io/greenballs/

## Unchecked snippets


* First
```groovy
def currentBuild = Thread.currentThread().executable
def env = currentBuild.getEnvironment()
String desc = """
  Commit: ${env['GIT_COMMIT'][0..8]}
"""
currentBuild.setDescription(desc)
```

* Second
```groovy
buildResult.isWorseOrEqualTo(Result.UNSTABLE)
```

* Third
```groovy
hudson.tasks.ArtifactArchiver
hudson.model.Fingerprint
TaskListener -> hudson.util.StreamTaskListener

def job = Hudson.instance.getJob("My Job Name")

FilePath OneOffExecutor.getCurrentWorkspace()
```

* Fourth

`git-ls-remote` in groovy:

```groovy
def gitURL = "https://github.com/grails/grails-core.git"
def command = "git ls-remote -h $gitURL"

def proc = command.execute()
proc.waitFor()              

if ( proc.exitValue() != 0 ) {
   println "Error, ${proc.err.text}"
   System.exit(-1)}

def branches = proc.in.text.readLines().collect { 
    it.replaceAll(/[a-z0-9]*\trefs\/heads\//, '') 
}

println br
```

* Unchecked pipelines

```groovy
library(
    identifier: "jakas-libka@${wersja}",
    retriever: legacySCM([
        $class: 'GitSCM',
        userRemoteConfigs: [[
           credentialsId: 'JAKIS_CREDENTIALS_ID',
           url: 'https://github.com/costam/costam.git'
]],
branches : [[name: "refs/tags/${wersja}"]],
extensions : [[ $class: 'CleanCheckout']]
])

pipeline {
agent {
label 'docker'
}
options {
disableConcurrentBuilds()
timeout(time: 45, unit: 'MINUTES')
}
environment {
JAKAS_ZMIENNA = 'jakas_wartosc'
JAKIS_KREDENCJAL = credentials('JAKIS TOKEN')
}
stages {
stage('To jest stage') {
steps {
script {
jakasfunkcja.z.jakiejs.libki()
}
}
}
```

przemycanie zmiennych miedzy krokami:

```groovy
env.ZMIENNA = 'wartosc'
env.ZMIENNA2 = sh(script: 'polecenie', returnStdout: true).trim()
currentBuild.displayName = "mozna dodawac" + ${lancuchy}

withCredentials([[$class: 'UsernamePasswordMultiBinding',
credentialsId: 'jakies-kredencjaly',
usernameVariable: 'USERNAME',
passwordVariable: 'PASSWORD'
]]) {
  echo 'do something'
 sh 'command $USERNAME $PASSWORD'
}

when {
beforeAgent true
expression {currentBuild.currentResult != 'ABORTED' }
anyOf {
  environment name: 'BRANCH_NAME', value: 'master'
  environment name: 'BRANCH_NAME': value: 'slave'
}
}
agent {
  docker {
      image 'image'
      reuseNode true
}
}
stages {
  stage('XYZ') {
  when {
   anyOf <- tu w zasadzie moze byc ten anyOf
}
}
steps, itd


post {
always {
script {
emailResult(toList="email@jakis")
}
cleanWs()
}
}
```
