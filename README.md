# nginx-rtmp-restream-server

## Docker + Nginx RTMP

### How to use  

OBS  -->  NginX Restream Server -->  Youtube , Twitch , Facebook , TikTok and more

### Install Reqirement 

````
sudo apt update
sudo apt install docker.io -y
sudo systemctl enable --now docker
````

### Create Dir

````
mkdir -p ~/nginx-rtmp
nano ~/nginx-rtmp/nginx.conf
````


### Add file 

````
worker_processes auto;
events { worker_connections 1024; }

rtmp {
    server {
        listen 1935;
        chunk_size 4096;

        application live {
            live on;

            # Your restream target
            # push rtmp://a.rtmp.youtube.com/live2/YOURKEY;
            # push rtmp://live.twitch.tv/app/YOURKEY;
        }
    }
}

http {
    server {
        listen 8080;

        location /stat {
            rtmp_stat all;
            rtmp_stat_stylesheet stat.xsl;
        }

        location /stat.xsl {
            root /usr/local/nginx/html/;
        }
    }
}
````

### Run Docker 

````
docker run -d --name nginx-rtmp \
  -p 1935:1935 \
  -p 8080:8080 \
  -v ~/nginx-rtmp/nginx.conf:/etc/nginx/nginx.conf \
  tiangolo/nginx-rtmp
````

###  check status 

````
docker ps
````
#### like this work !!!
````
Up ... 0.0.0.0:1935->1935/tcp, 0.0.0.0:8080->8080/tcp nginx-rtmp
````


### Stream by OBS 

````
rtmp://YOUR-IP/live
key: test
````





