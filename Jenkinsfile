pipeline {
agent any

```
tools {
    maven 'maven'
    jdk 'JAVA_HOME'
}

stages {
    stage('Clone') {
        steps {
            git branch: 'main', url: 'https://github.com/lindaismail2/student-management.git'
        }
    }

    stage('Build') {
        steps {
            sh 'mvn clean package -DskipTests'
        }
    }

    stage('Archive') {
        steps {
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
    }
}

post {
    always { echo 'Pipeline terminé' }
}
```

}
