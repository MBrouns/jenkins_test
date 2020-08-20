pipeline {
    agent {
         docker { 
	     image 'rocker/r-ver:4.0.0' 
             args '-p 8000 '
         }
    }
    environment {
        R_LIBS = "${env.WORKSPACE}/.rlib"
    }
    stages {
        stage('Build environment') {
            steps {
                sh '''
		apt-get update -qq && apt-get -y --no-install-recommends install  libssl-dev libxml2-dev libcurl4-openssl-dev libssh2-1-dev unixodbc-dev libsasl2-dev 
		mkdir -p ${R_LIBS}
		Rscript -e "install.packages('renv', lib=Sys.getenv('R_LIBS'))"
		Rscript -e "renv::init()"
		Rscript -e "renv::restore()"
		Rscript -e "install.packages('devtools', lib=Sys.getenv('R_LIBS'))"
	    	Rscript -e "install.packages('.', repos = NULL, type='source' , lib=Sys.getenv('R_LIBS'))"     
          	chmod +x R/cli.R
		Rscript -e "devtools::check()"
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
                sh './R/cli.R --bind 0.0.0.0'
                input message: 'version looks ok?'

            }
        }
    }
}
