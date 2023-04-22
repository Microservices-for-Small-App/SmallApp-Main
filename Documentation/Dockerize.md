# Docikerize your microservices

## 1. Build the Docker Image using Docker CLI and PowerShell

### 1.1. Export required Variables using PowerShell Windows Terminal

```powershell
$env:GH_OWNER="Microservices-for-Small-App"
$env:GH_PAT="ghp_Your_GitHib_Classic_PAT"
```

### 1.2. Execute the below mentioned Docker Command(s) in PowerShell Windows Terminal to create the Docker Image

```powershell
cd C:\LordKrishna\SSP\Services-PlayIdentity

docker build --secret id=GH_OWNER --secret id=GH_PAT --pull --rm -f "./Src/Identity.Service/Prod.Dockerfile" -t identityservice:$(Get-Date -Format yyyyMMddHHmmssfff) -t identityservice:latest .
```

![Build Docker Image Locally |150x150](./Images/Dockerize/Build_Image_Locally_Identity.PNG)

### 1.3. Execute the below mentioned Docker Command(s) in PowerShell Windows Terminal to RUN Docker Container

```powershell
$adminPass="Sample@123$"
docker run -it --rm -d -p 5002:5002 --name identity -e MongoDbSettings__Host=mongo -e RabbitMQSettings__Host=rabbitmq -e IdentitySettings__AdminUserPassword=$adminPass --network dakar_default identityservice:latest
```

![Run Docker Container Locally |150x150](./Images/Dockerize/Run_Container_Locally_Identity.PNG)
