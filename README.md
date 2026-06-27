# JavaHelloWorldJar

****🛠 Create a systemd Service File
###################Create service file ###################################
****

bash: # sudo nano /etc/systemd/system/<hello-world.service>
**Add configuration  : Paste this content (adjust paths and JAR name as needed):**

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
ExecStart → path to your JAR file.

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
