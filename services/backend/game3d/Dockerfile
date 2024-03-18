FROM debian:latest

### Necessary dependencies for Django
RUN apt update -y
RUN apt install python3 python3-pip -y
RUN apt install postgresql -y

# Clean lists to reduce size of container
RUN rm -rf /var/lib/apt/lists/*

### Env variables assignation (to fix)
ENV GAME_DB_USER=dbking
ENV GAME_DB_PASSWORD=kingp4bl0
ENV GAME_DB_BASENAME=testbase

# Copy our configuration
COPY /conf/pg_hba.conf /etc/postgresql/15/main/pg_hba.conf
RUN chmod 777 /etc/postgresql/15/main/pg_hba.conf

### Database basic initialisation
COPY /conf/init_db.sh .
RUN chmod 777 init_db.sh
RUN ./init_db.sh

### Django and other python packages installation
COPY /conf/requirements.txt .
RUN pip install --break-system-packages -r requirements.txt

### Copy django app files in container
WORKDIR /app
COPY  /tools/game3d/ /app/
COPY /conf/start.sh .
RUN chmod 777 start.sh

### Expose port
EXPOSE 8080
EXPOSE 5433

### Launch game application
ENTRYPOINT [ "sh", "start.sh" ]