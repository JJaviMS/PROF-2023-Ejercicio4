pipeline {
    agent any 
    stages {
        stage('Backup') { 
            steps {
                sh 'cp /database/Employees.db /database/Employees.db.bk'
                sh 'sqlite3 /database/Employees.db ".dump" | grep INSERT > /database/data.sql' // Extract the data
            }
        }
        stage('Delete schema') { 
            steps {
                sh 'rm /database/Employees.db' // Delete the db
            }
        }
        
        stage('Load schema'){
            steps {
                sh 'sqlite3 /database/Employees.db < sqlite.sql'
            }
        }
        
        stage('Restore data'){
            steps {
                sh 'sqlite3 /database/Employees.db < /database/data.sql'
            }
        }
        
    }
    
    post{
        unsuccessful{
            sh 'mv /database/Employees.db.bk /database/Employees.db'
        }
        
        cleanup {
            sh 'rm /database/Employees.db.bk'
            sh 'rm /database/data.sql'
        }
    }
}