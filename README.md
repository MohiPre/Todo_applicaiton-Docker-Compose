Using Docker Compose to Run Your Applications

--> Docker Compose is an amazing tool to create the development environment for your application stack. It allows you to define each component of your application following a clear and simple syntax in YAML files.

--> I have build a TODO applicaiton ( Frontend[REACT] - Backen[NODE] - Database[MondoDB] )

--> First always go via the imperatinve approch for a Multi-Container Applicaiton and then go for Declerative approach .
```
Below is my TODO Applicaiton 
------------------------------

version: "3"
services:

  mongodb:                                   #Creating Database
    image: mongo                             # pulling image from dockerhub
    container_name: mongodb
    env_file:                                #env file for MongoDB authentication
      - ./env/mongodb.env
    ports:
      - "27017:27017"
    volumes:
      - mydbvol:/db/data                     #Persistent Volume for db

  mongoexp:                                  # Frontend GUI for database
    image: mongo-express                     # pulling image from dockerhub
    ports:                                   # portmapping
      - "8081:8081"
    env_file:                                #env file for MongoDB authentication
      - ./env/exp.env
    restart: always
    container_name: mongoexpress
    depends_on:
      - mongodb


  backend:                                   # backend API
    build: ./backend                         # Building image form Dockerfile
    ports:                                   # Port Mapping
      - "8000:8000"
    volumes:                                 # Bind Mount
      - ./backend/src:/app/src
    env_file:                                # Backend Api Auth via env file
      - ./env/backend.env
    depends_on:
      - mongodb
    container_name: backend
    restart: always

  frontend:
    build: ./frontend                        # Building a image
    stdin_open: true                         # -it is short for --interactive + --tty. When you docker run with this command it takes you straight inside the containe 
    tty: true
    ports:                                   # PortMapping
      - "3000:3000"
    volumes:                                 # Bind Mount
      - ./frontend/src:/app/src
    depends_on:
      - backend
      - mongodb
    restart: always

volumes:
  mydbvol:
```



