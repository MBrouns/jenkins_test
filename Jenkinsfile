pipeline {
    agent {
        docker { image 'rocker/r-ver:4.0.0' }
    }
    stages {
        stage('Test') {
            steps {
                sh 'Rscript -e renv::init()'
            }
        }
    }
}
