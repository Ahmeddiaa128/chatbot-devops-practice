# chatbot-devops-practice
1️⃣) Istall python-dotenv
This library lets FastAPI read values from a .env file.

$ pip install python-dotenv
=============================================================
2) Create a .env file in your project folder
In the same directory where main.py (or app.py) is:

$ nano .env
$ OPENAI_API_KEY=sk-XXXXXXXXXXXXXXXXXXXXXXXXXXXX
Step 1 — Create the Secret
Run this in your k8s folder:
$ kubectl create secret generic openai-secret \
    --from-env-file=.env
Verify:
$ kubectl describe secret openai-secret
You should see:
OPENAI_API_KEY
==============================================================
$ eval $(minikube docker-env)
$ docker build -t diaa-chatbot:latest .
Then K8s can use it directly.

Option B — Docker Hub
bash
Copy code
$ docker tag diaa-chatbot:latest yourdockerhubusername/diaa-chatbot:latest
$ docker push yourdockerhubusername/diaa-chatbot:latest
#############################################
🟢 2️⃣ <pending> is NORMAL in Minikube

Minikube does not have a real cloud load balancer.
So EXTERNAL-IP will always stay:

<pending>


This is expected and not an error.

🟢 3️⃣ How to access the service in Minikube

Use:

$ minikube service chatbot-service


It will output something like:

http://192.168.49.2:31740


That is your public endpoint.
############################################
🧱 How Kubernetes networking really works

Your stack becomes:
-------------
Internet
   ↓
Nginx Ingress
   ↓
Kubernetes Service (chatbot-service)
   ↓
Chatbot Pods (2 replicas)
------------------

So:

Service = internal load balancer between pods

Ingress = HTTP gateway from the outside world

They solve different problems.

❌ Why you should NOT remove the Service

Ingress cannot talk directly to pods.
Ingress only routes traffic to Services.
###################################
When you want to craete ingess service as a collector ingress to you app .
you should make the type of service as ClusterIP not LoadBalancer Because Nginx will become the public entry point.

🧪 Test Ingress

Get Minikube IP:

$ minikube ip


Edit your local hosts file:

$ sudo nano /etc/hosts


Add:
$ <MINIKUBE-IP> chatbot.chatbot.example.com

