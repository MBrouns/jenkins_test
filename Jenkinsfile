pipeline {
    agent any

    stages {
        stage('Build environment') {
            steps {
                sh '''
		Rscript -e "install.packages('renv')"
		Rscript -e "renv::init()"
		Rscript -e "renv::restore()"
		Rscript -e "install.packages('devtools')"
		'''
            }
        }
        stage('Test') {
            steps {
                sh '''
		Rscript -e "devtools::check()"
		'''
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
