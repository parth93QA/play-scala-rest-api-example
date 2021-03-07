pipeline {
  agent any
  stages {
    stage('Compile') {
      steps {
        echo 'Compiling...'
        sh "${tool name: 'sbt', type:'org.jvnet.hudson.plugins.SbtPluginBuilder$SbtInstallation'}/bin/sbt compile"
        sh 'echo "${COMPILER}"'
      }
    }

    stage('Packaging') {
      steps {
        echo 'Packaging...'
        sh "${tool name: 'sbt', type:'org.jvnet.hudson.plugins.SbtPluginBuilder$SbtInstallation'}/bin/sbt clean stage"
      }
    }

    stage('error') {
      steps {
        archiveArtifacts(artifacts: 'target/**/*.jar', fingerprint: true)
      }
    }

  }
  environment {
    COMPILER = 'sbt'
  }
}