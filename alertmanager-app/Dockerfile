# Use a imagem do Ubuntu como base
FROM ubuntu:latest

# Atualize a lista de pacotes e instale o Python 3 e outras dependências
RUN apt-get update -y && \
  apt-get install -y python3-pip python3-dev default-libmysqlclient-dev && \
  apt-get clean

# Crie um diretório de trabalho para o projeto
WORKDIR /app

# Copie o arquivo requirements.txt para o diretório de trabalho
COPY requirements.txt .

# Copie o diretório do projeto para o diretório de trabalho
COPY . . 

# Instale as dependências do projeto
RUN pip3 install --upgrade pip && \
  pip3 install -r requirements.txt

# Copie o código-fonte do projeto para o diretório de trabalho
COPY . /app

# Configure o banco de dados MySQL
ENV MYSQL_DATABASE=reports \
  MYSQL_USER=newuser \
  MYSQL_PASSWORD=password \
  MYSQL_HOST=localhost \
  MYSQL_PORT=3306

# Instale o servidor MySQL e crie o banco de dados
RUN apt-get update -y && \
  apt-get install -y mysql-server && \
  service mysql start && \
  mysql -u root -e "CREATE DATABASE reports" && \
  mysql -u root -e "CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password'" && \
  mysql -u root -e "GRANT ALL PRIVILEGES ON reports.* TO 'newuser'@'localhost'" && \
  mysql -u root -e "USE reports; CREATE TABLE alertas ( id INT AUTO_INCREMENT PRIMARY KEY, status VARCHAR(100) NOT NULL, alertname VARCHAR(100) NOT NULL, instance VARCHAR(100) NOT NULL, job VARCHAR(100) NOT NULL, severity VARCHAR(100) NOT NULL, startsAt VARCHAR(100) NOT NULL, endsAt VARCHAR(100) );" && \
  mysql -u root -e "FLUSH PRIVILEGES"


# Expõe a porta do Flask
EXPOSE 5000

# Define o comando para iniciar o servidor Flask
CMD ["python3", "-u", "-t", "app.py", "tail", "-f", "/dev/null"]