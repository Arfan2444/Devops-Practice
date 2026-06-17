pipeline {
    agent any

    environment {
        FRONTEND_IMAGE = "mern-frontend:jenkins"
        BACKEND_IMAGE  = "mern-backend:jenkins"
        PORT           = "5000"
        MONGO_URI      = "mongodb://mongo:27017/taskdb"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/Arfan2444/Devops-Practice',
                    branch: 'main'
            }
        }

        stage('Prepare Env') {
            steps {
                sh '''
                mkdir -p server

                cat > server/.env <<EOF
PORT=$PORT
MONGO_URI=$MONGO_URI
EOF

                cat server/.env
                '''
            }
        }

        stage('Build Docker Images') {
            steps {
                sh '''
                echo "Building backend image..."
                docker build -t $BACKEND_IMAGE ./server

                echo "Building frontend image..."
                docker build -t $FRONTEND_IMAGE \
                    --build-arg VITE_API_URL=http://localhost:$PORT/api \
                    ./client
                '''
            }
        }

        stage('Run Containers') {
            steps {
                sh '''
                echo "Starting services..."
                docker compose up -d

                echo "Running containers:"
                docker ps

                echo "Backend logs:"
                docker logs backend || true

                echo "Frontend logs:"
                docker logs frontend || true
                '''
            }
        }
    }
}