# selfie-less-App
photo share web app

put users and database folder in /home/ubuntu of users instance
put acts,database1,fault_tolerant.py and auto_scaling.py in /home/ubuntu of acts instance

change team id in docker file of both users and acts


users instance:
sudo apt-get install sqlite3
cd users
sudo docker build -t "users" .
sudo docker run -p 80:5000 -v /home/ubuntu/database:/data users



acts instance:

	sudo apt-get install sqlite3
	sudo apt-get install nginx 
---------------------------------------------	
	touch /etc/nginx/conf.d/default.conf
		#edit the below lines file /etc/nginx/conf.d/default.conf
		upstream my-app{
		 server  localhost:8000 weight=1;
		}

		server{
		        location / {
		                proxy_pass http://my-app;
		        }
		}

 -------------------------------------------------------------

	sudo systemctl restart nginx

	open 3 terminal of acts instance

	terminal1:
	cd acts
	sudo docker build -t "acts" .
	sudo docker run -p 8000:80 -v /home/ubuntu/database1:/data acts

	terminal2:
	python3 auto_scaling.py

	terminal3:
	python fault_tolerant.py
