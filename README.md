# Simulated-Secure-Production-Network
A Small Simulated Secure Production Network on Azure 

Goal: Build a small, simulated environment in Azure that mimics a secure production network:
- A Virtual Network (VNet) for isolation
- Two Virtual Machines (VMs) running Docker
- A load Balancer distributing traffic between the,
- A DVWA (Damn Vulnerbale Web App) container running on each VM
- Security Monitoring, Traffic Blocking, and logging to analyze attacks
---------------------------------------------------------------------------------------

Defined Terms: 
Virtual Network - is a user's own private network in a cloud or on-premises environment, created using software (Azure) that allows for the logical sepearation and secure communication of resources without physical hardware 

Virtual Machine - a software-based, (Azure) virtual computer that runs on a physical computer, or "host". It uses the host's hardware resources like CPU, Memory, and Storage, allowing you to run an operating systems nad applications in an isolated environemnt, seperate froom the host's primary system.

Load Balancer - is a hardware or software device that distributes incoming network traffic across a group of backend servers to ensure high avaliability and prevent any single server from becoming overloaded. This acts as a "traffic cop" per say, routing client requests to avaliable servers, which improves application responsiveness and performance

Damn Vulnerable Web App - is a PHP/MySQL web application that is intentionally built with numerous security flaws. It servers as a safe and legal environment for security professionals, developers. 

Security Monitoring - the continuous process of collecting analyzing, and responding to security-related information to detect threats in real time. 

Traffic Blocking - the process of preventing data from reaching its destination by using security devices like firewalls, which filter network traffic based on predefined rules. 
-----------------------------------------------------------------------------------------------

Step-by-Step Breakdown: 

1. Set up the Azure Infrastructure:
   - Create a Resource Group (e.g. DVWA-Lab)
   - Create a Virtual Network (VNet) with at least two subnets:
     - frontend-subnet for the load balancer
     - backend-subnet ofr your VMs
   - Deploy two Linux VMs (Ubuntu for Docker):
     - Place them both in the backend subnet

2. Install Docker on the VMs:
   - SSH into each VM and install Docker;
     sudo apt update
     sudo apt install -y docker.io
     sudo systemctl enable docker
     sudo systemctl start docker
  - Then pull and run DVWA:
    - sudo docker run -d -p 80:80 vulnerables/web-dvwa
  - Check that it's running:
    - docker ps

3. Add the Azure Load Balancer:
   - Deploy an Azure Load Balancer (Basic or Standard)
   - Configure a Backend Pool with your two VMs
   - Add a Health Probe (HTTP on port 80)
   - Create a Load Balancer Rule for port 80 (HTTP)
 - DVWA app should be accessible via the Load Balancer's public IP


4. Configure Network Security:
- User Network Security Groups (NSGs):
  - Allow only necessary ports (e.g., Port 22 for SSH, Port 80 for HTTP etc.)
  - Deny other inbound traffic
  - Try experimenting by blocking or allowing certain IPs
  - Deploy Azure Firewall or Application Gateway (WAF)

5. Enable Logging & Monitoring:
   - Azure Monitor / Log Analytics Workspace - collect logs
   - NSG flow logs -> See allowed/denfied traffic
   - VM logs -> View Docker Access Attempts
  
6. Options Ideas:
   - Add Azure Bastion to securely connect via SSH (no public IPs on VMs)
   - Add a Network Watched Topology View
   - Use Security Center (Defender for Cloud) to detect vulnerabilities in the DVWA setup
   - Try running nmap, nikto, or Burp Suite scans againt your DVWA load balancer
  
        Internet
            |
       [Load Balancer]
            |
      ┌───────────────┐
      |     VNet      |
      |  ┌─────────┐  |
      |  |  VM1     |  |
      |  |  Docker  |  |
      |  └─────────┘  |
      |  ┌─────────┐  |
      |  |  VM2     |  |
      |  |  Docker  |  |
      |  └─────────┘  |
      └───────────────┘
