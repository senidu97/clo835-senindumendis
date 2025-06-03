# CLO835-Assignment 1 – Flask App with MySQL (Dockerized) by senindumendis.

This project runs a Flask web application and a MySQL database in separate Docker containers on a shared Docker network.

---

## 🧱 Project Structure

```
├── Dockerfile # App container Dockerfile
├── app.py # Flask application
├── requirements.txt # Python dependencies
├── db/
│ ├── Dockerfile_mysql # MySQL container Dockerfile
│ └── mysql.sql # SQL to initialize database
└── templates/ # HTML templates
```

### Building mysql docker image 
```docker build -t clo835-db -f "db/Dockerfile" . ```

### Building application docker image 
```docker build -t clo835-app -f Dockerfile . ```

### Creating Network

```docker network create clo835-network ```

### Running mysql DB on port 3306 in the same network created
```docker run -d -p 3306:3306 --network=clo835-network -e MYSQL_ROOT_PASSWORD=pass@1234 --name=clo835-db clo835-db ```

### Exporting the values for the app
```
export DBHOST=clo835-db
export DBPORT=3306
export DBUSER=root
export DATABASE=employees
export DBPWD=pass@1234
```
### Run the application where the background is blue on port 8081

```docker run -d -p 8081:8080 --network=clo835-network -e DBHOST=$DBHOST -e DBPORT=$DBPORT -e  DBUSER=$DBUSER -e DBPWD=$DBPWD -e APP_COLOR=blue --name=clo835-app-blue clo835-app```

### Run the application where the background is pink on port 8082

```docker run -d -p 8082:8080 --network=clo835-network -e DBHOST=$DBHOST -e DBPORT=$DBPORT -e  DBUSER=$DBUSER -e DBPWD=$DBPWD -e APP_COLOR=pink --name=clo835-app-pink clo835-app```

### Run the application where the background is lime on port 8083

```docker run -d -p 8083:8080 --network=clo835-network -e DBHOST=$DBHOST -e DBPORT=$DBPORT -e  DBUSER=$DBUSER -e DBPWD=$DBPWD -e APP_COLOR=lime --name=clo835-app-lime clo835-app```


### Pull images from AWS ECR

### Login

```aws ecr get-login-password --region <your-region> | docker login --username AWS --password-stdin <your-account-id>.dkr.ecr.<your-region>.amazonaws.com```

### Pull the images

```docker pull <your-account-id>.dkr.ecr.<your-region>.amazonaws.com/clo835-db:latest```
```docker pull <your-account-id>.dkr.ecr.<your-region>.amazonaws.com/clo835-app:latest```

### Run the containers

```docker run -d -p <dedicated port> --network=clo835-network -e DBHOST=$DBHOST -e DBPORT=$DBPORT -e  DBUSER=$DBUSER -e DBPWD=$DBPWD -e APP_COLOR=blue  <your-account-id>.dkr.ecr.<your-region>.amazonaws.com/clo835-app:latest --name=<app-name>```
