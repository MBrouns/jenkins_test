pipeline {
    agent {
         docker { 
	     image 'rocker/r-ver:4.0.0' 
             args '-p 8000 -u 0:0'
         }
    }
    environment {
        R_LIBS = '~/.rLibs'
    }
    stages {
        stage('Build environment') {
            steps {
                sh '''
		
		Rscript -e "install.packages('renv', lib=Sys.getenv('R_LIBS'))"
		Rscript -e "renv::init()"
		Rscript -e "renv::restore()"
		Rscript -e "install.packages('devtools', lib=Sys.getenv('R_LIBS'))"
	    	Rscript -e "install.packages('.', repos = NULL, type='source' , lib=Sys.getenv('R_LIBS'))"     
          	chmod +x R/cli.R
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
