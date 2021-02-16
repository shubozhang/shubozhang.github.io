






# Sonarqube


```bash
version: '2'
services:
  postgres:
    image: postgres:9.6
    networks:
      - sonarqube
    environment:
      POSTGRES_USER: sonar
      POSTGRES_PASSWORD: sonarpasswd
    volumes:
      - ~/postgresql/data
  sonarqube:
    image: sonarqube:7.9.5-community
    ports:
      - "9000:9000"
      - "9092:9092"
    networks:
      - sonarqube
    environment:
      SONARQUBE_JDBC_USERNAME: sonar
      SONARQUBE_JDBC_PASSWORD: sonarpasswd
      SONARQUBE_JDBC_URL: "jdbc:postgresql://postgres:5432/sonar"
      SONAR_SEARCH_JAVAADDITIONALOPTS: "-Xmx4096m -Xms2048m"
      SONAR_SCANNER_OPTS: "-Xmx4096m"
    depends_on: 
      - postgres

networks:
  sonarqube:
```
