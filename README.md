# snapshot

Please note that you can have as many docker compose.yml as you need. There is no limit other that your system resource. 

Setup your Linux box as it needs to be. I used Ubuntu 22.04 and I have completed the apt update
You will need to install docker
 sudo apt install docker
Sudo apt install docker-compose
Do a simple check on the docker version by 
Docker – version 
Docker run hello-world 

You can remove your hello-world by doing the following 
Docker compose stop hello world which is the name of the container
To Run your docker setup, you will need to create a docker-compose file. 
Go to /opt/ 
Mkdir polkadot or polkadot_validator (I use polkadot_validator) 
Cd polkadot_validator 
nano /vi docker-compose.yml 
I will attach a copy of this on my github repo. 
Let me explain through the code 
Version: ‘3’
This is reference to docker-compose.yml version 3.x, this is for syntax only. You can reference it to the version in the docker compose website. 


version: '3'        
# This is a reference to docker-compose.yml version 3.x, this is for syntax only. You can reference it to the version in the docker compose website.   

services:
  polkadot_node1:
# This is the name of the container that we will be creating, as an identifier for your worry, usually we have the same name here and the container name. 
    container_name: polkadot_node1
# This is the name of the container that we will be creating.

    image: parity/polkadot:latest
# You can use latest in this to get the latest version, however, it is your call to see if you want to use a steady version or the latest version of the parity polkadot.  
    ports:
      - 30333:30333 # p2p port - this is standard P2P port
      - 9933:9933 # rpc port - Standard RPC Port 
      - 9944:9944 # ws port - standard WS port 
    command: [
      "--name", "your_validator_name",
      "--rpc-cors", "all",
      "--pruning", "1000",
      “--unsafe-pruning”
      “--validator”,
      "--chain", “polkadot"
      “--telemetry-url”, “wss://telemetry.polkadot.io/submit/ 1”
    ]
    deploy:
      resources:
        limits:
          memory: 5120m
          Cpu: 0.50
        reservations:
          memory: 2048m
    volumes:
      - /data/polkadot_node1:/polkadot
    

You can also limit the number of CPU by doing on the limit 
Cpu: 0.50 
0.50 = 50% of 1 cpu, if you want 5.0 for CPU, that’s 5 vcpu allocated. 

Make sure that you save the file and verify again with cat docker-compose.yml 
Go to /data and perform the following command to assure that you have the right user access. This is just an example folder, you can put it anywhere you want. 
Chown 1000:1000 /data -R 
Go to /opt/polkadot_validator folder

Docker compose up

You will see that your docker is creating the container and it will show the logs directly on your screen. That’s because we are running it for the first time for “review” if there are any error. 
If there is no error and the container is synching to the chain, you are good and you can control + Z to get out of the process. It will stop the container. 
Type the following command to run this in the background
Docker compose up -d
This time, you will see that th container are being built  
Docker compose down to remove the container

Some useful commands are:
docker compose up
docker compose up <service_name>
docker compose up -d  # this is to run in the background
docker compose up -d --no-recreate 
# this will bring all node up without changing the container ID
docker compose down
docker compose down <service_name>
docker compose logs
docker compose logs <service_name>
Docker compose stop <service_name>
Docker compose start <service_name>
docker compose logs -f = for incoming new logs 

docker compose logs --since 2022-09-24 | grep polkadot
