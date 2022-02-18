# Intelie_ssh_challenge
#Here you will see the step by step of how to connect to a server specific port, passing through a jumphost

After setting up a lab on virtual-box software, installing openssh-server, starting the ssh service, enabling port fowarding on the sshd_config file on all VMs. 
My lab is composed by 3 VMs:

lianmacedo1-VirtualBox | IP: 10.0.2.15 | named as Ubuntu-1 on VirtualBox  (Local)

lianmacedo2-VirtualBox | IP: 10.0.2.6 | named as Ubuntu-2 on VirtualBox (jump)

lianmacedo3-VirtualBox | IP: 10.0.2.4 | named as Ubuntu-3 on VirtualBox (server)

Before we can resolve the challenge in a single command, we will resolve it step by step. The main task is to connect from the lianmacedo1 to lianmacedo2 and to the port 8000 on lianmacedo3.

The first step is to connect to the lianmacedo2 via SSH from the lianmacedo1.
We will run the following command: "ssh lianmacedo2@10.0.2.6", after typing the password for the lianmacedo2 user it will connect to the lianmacedo2-VirtualBox as the user lianmacedo2.
#If we use a user that not exists in the lianmacedo2 we will receive a authentication error.

![image](https://user-images.githubusercontent.com/97834655/154605399-57215b26-8861-4294-9ec8-d5ffe5875895.png)


The second step is to connect to the lianmacedo3 via SSH from the lianmacedo2. We should execute the command "ssh lianmacedo3@10.0.2.4". After typing the password for the lianmacedo3 user, we are in the lianmacedo3-VirtualBox!

<img width="955" alt="image" src="https://user-images.githubusercontent.com/97834655/154601273-7fecc854-6ad1-411e-8a9a-55b24a06cfba.png">

We now know that we have connection between the 3 VMs, connecting one to one. So let's make a jump using the -J option of the ssh command to connect from lianmacedo1, passing to the lianmacedo2 and reaching lianmacedo3. We will execute the command "ssh -J lianmacedo2@10.0.2.6 lianmacedo@10.0.2.4". After inserting the password for each VM we should connect from the lianmacedo1 to the lianmacedo3.

<img width="955" alt="image" src="https://user-images.githubusercontent.com/97834655/154603931-1633c15e-c991-4517-98e8-471c4aae9bb3.png">

Let's "exit" from the lianmacedo3, returning to the lianmacedo1 and connect to the lianmacedo2 again. 

Once we are in the lianmacedo2 we should connect to the port 8000 of the lianmacedo3, to make the port fowarding to the port 8000 we need to execute the command "ssh lianmacedo3@10.0.2.4 -L 8080:10.0.2.4:8000". 
#We are using the port 8080 as the localhost port to access the port 8000 on the lianmacedo3, but we could use other port number.

![image](https://user-images.githubusercontent.com/97834655/154602651-1f6fd5bc-a3df-4851-91af-bbafc79ebc3e.png)

Opening the browser on the lianmacedo2 and making a request to the 8080 port we should get some tries on the terminal of lianmacedo1, once we are inside lianmacedo3. 

<img width="955" alt="image2" src="https://user-images.githubusercontent.com/97834655/154606589-fcc3a697-3163-4831-9d7b-0b4358dc9f42.png">
<img width="955" alt="image" src="https://user-images.githubusercontent.com/97834655/154606657-9ba0c493-a514-4186-a280-4344509c8283.png">
#There is no service running on the port 8000 on the lianmacedo3, so we will receive the "Name or service not know" message.

We resolved the challenge, but we needed to type several commands, step by step! 

Let's resolve it in one line! To execute it we will gather the command above and insert the -J option of the ssh command we used earlier. So we will get the final command: "ssh -J lianmacedo2@10.0.2.6 lianmacedo3@10.0.2.4 -L 8080:lianmacedo3@10.0.2.4:8000"

![image](https://user-images.githubusercontent.com/97834655/154604924-96c53fbb-277d-4d5f-bb39-234d98bb553a.png)

We have connected from the lianmacedo1 to the port 8000 on the lianmacedo3! To confirm it we will open the browser on the lianmacedo1 and make a request for the port 8080.
![image](https://user-images.githubusercontent.com/97834655/154605180-86b61fcd-a092-4c49-88cb-e946b19b5050.png)

Checking on the terminal we will get: 
![image](https://user-images.githubusercontent.com/97834655/154605232-b5c21745-b0be-4a63-b0bf-1a9ee8d62b77.png)

#There is no service running on the port 8000 on the lianmacedo3, so we will receive the "Name or service not know" message.

