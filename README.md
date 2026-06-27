# JavaHelloWorldJar

##################🛠 Create a systemd Service File#####################
###################Create service file ###################################

bash: # sudo nano /etc/systemd/system/<hello-world.service>
**Add configuration  : Paste this content (adjust paths and JAR name as needed):**

<img width="1026" height="543" alt="image" src="https://github.com/user-attachments/assets/bfdb69d9-a65f-4556-98f0-7e3e61fd97a7" />


[Unit]
Description=Hello World Java App
After=network.target

[Service]
User=ec2-user
ExecStart=/usr/bin/java -jar /home/ec2-user/app/hello-world-1.0-SNAPSHOT-shaded.jar
SuccessExitStatus=143
Restart=always
StandardOutput=append:/home/ec2-user/app/app.log
StandardError=append:/home/ec2-user/app/app.log

[Install]
WantedBy=multi-user.target


####################################################
User → the Linux user running the app (often ec2-user).
Logs will be written to /home/ec2-user/app/app.log.

###################################
🔑 **Enable and Control the Service**:
###################################

1: #### Reload systemd:
bash: sudo systemctl daemon-reload

2: #### Enable service at boot
bash : sudo systemctl enable hello-world

3: #### Start service
bash: sudo systemctl start hello-world

4: #### Check status
bash: sudo systemctl status hello-world

5: #### Stop service
bash: sudo systemctl stop hello-world

6: #### Restart service
bash: sudo systemctl restart hello-world

######################################################################
📋 **Verify in Browser**
Ensure your EC2 security group inbound rules allow traffic on port 8080.

Open:

http://<EC2-public-ip>:8080
You should see your Hello World response.

######################################################################
⚡ **Best Practices**

Keep logs in /var/log/ for production apps.
Use Restart=always so the app auto-restarts if it crashes.
For HTTPS, add Nginx reverse proxy in front of your JAR.


###################################################
        🛠 Install Nginx on EC2
###################################################
sudo yum install -y nginx   # Amazon Linux 2
sudo systemctl enable nginx
sudo systemctl start nginx


**🔑 Configure Reverse Proxy:**

sudo nano /etc/nginx/conf.d/hello-world.conf

###################################################################

<img width="1079" height="565" alt="image" src="https://github.com/user-attachments/assets/7ea1b8fe-0b81-426d-92ed-81820ab4b98d" />



server {
    listen 80;
    server_name _;

    location / {
        proxy_pass http://127.0.0.1:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
#######################################################################
Note:
listen 80 → Nginx listens on port 80 (HTTP).

proxy_pass → forwards requests to your JAR running on port 8080.


sudo nginx -t
sudo systemctl restart nginx
