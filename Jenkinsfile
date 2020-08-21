pipeline {
    agent any
    environment {
	HOME = "${env.WORKSPACE}"
        R_LIBS = "${env.WORKSPACE}/.rlib"
    }
    stages {
        stage('Build environment') {
            agent {
                docker {
                    image 'rocker/r-ver:4.0.0'
                    args '-u 0:0'
                }
     	    }
	    steps {
                sh '''
		apt-get update -qq && apt-get -y --no-install-recommends install libgit2-dev libssl-dev libxml2-dev libcurl4-openssl-dev libssh2-1-dev unixodbc-dev libsasl2-dev 
		Rscript -e "install.packages('renv')"
		Rscript -e "renv::init(force = TRUE)"
		Rscript -e "renv::restore(repos = 'https://packagemanager.rstudio.com/all/__linux__/focal/latest')"
		Rscript -e "install.packages('devtools')"
	    	Rscript -e "install.packages('.', repos = NULL, type='source')"     
          	chmod +x R/cli.R
		'''
            }
        }
        stage('Test') {
            agent {
                docker {
             	    image 'rocker/r-ver:4.0.0'
             	    args '-u 0:0'
                }
            }
	    steps {
                sh '''
		Rscript -e "options(testthat.output_file = "test-out.xml"); devtools::test(reporter = 'junit')"
		'''
            }
        }
        stage('Deploy') {
	    agent any
            steps {
                sh '''
                    docker build -t foo .
                '''
            }
        }
    }
    post {
        always {
	    junit 'test-out.xml'
        }
    }
}
