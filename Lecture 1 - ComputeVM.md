# Compute Engine - VM Instance - By Lakshika Tanwar
---
## Method 1 - Using GUI
We create the VM Instance using the GUI exploring N and E Seies machines.

## Method 2 - Using Google Cloud Console

### The following code is from our instance which we created using GUI. We used its --project and --service-account value and replaced those in the command provided by faculty.

```
gcloud compute instances create myvm1 --project=industrial-glow-437012-i4 --zone=us-central1-f --machine-type=g1-small --network-interface=network-tier=PREMIUM,stack-type=IPV4_ONLY,subnet=default --maintenance-policy=MIGRATE --provisioning-model=STANDARD --service-account=964926729963-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/trace.append --tags=http-server,https-server,lb-health-check --create-disk=auto-delete=yes,boot=yes,device-name=myvm1,image=projects/ubuntu-os-cloud/global/images/ubuntu-2004-focal-v20240830,mode=rw,size=10,type=pd-balanced --no-shielded-secure-boot --shielded-vtpm --shielded-integrity-monitoring --labels=goog-ec-src=vm_add-gcloud --reservation-affinity=any
```

---
### The following is the code provided by faculty with the above values replaced:
```
gcloud compute instances create myvm1 --project=industrial-glow-437012-i4 --zone=asia-east1-c --machine-type=e2-medium --network-interface=network-tier=PREMIUM,stack-type=IPV4_ONLY,subnet=default --maintenance-policy=MIGRATE --provisioning-model=STANDARD --service-account=964926729963-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/trace.append --tags=http-server --create-disk=auto-delete=yes,boot=yes,device-name=myvm1,image=projects/ubuntu-os-cloud/global/images/ubuntu-2204-jammy-v20240720,mode=rw,size=10,type=projects/golden-sentry-430013-j5/zones/asia-east1-c/diskTypes/pd-balanced --no-shielded-secure-boot --shielded-vtpm --shielded-integrity-monitoring --labels=goog-ec-src=vm_add-gcloud --reservation-affinity=any
```

We ran the above code in 'console' of gc. Once the Machine was deployed we used the following code to run the repository update and install apache2. Further, we created the index.html and deployed it live using public IP. (For some reason unknown the page was not accessible even with correct permissions).
```
apt update
apt -y install apache2
cat <<EOF > /var/www/html/index.html
<html><body><p>Linux startup script added directly. $(hostname -f) </p></body></html>
```

---
Once deployed, we can use the following code to access the list of VM's deployed. 
```
gcloud  compute  instances list
```

---

The following code is used to ssh the vm using google cloud console with the name as VM_NAME(replace with suitable name).
```
cloud compute ssh VM_NAME --project=replace_withyour_project_id --zone=asia-east1-c
```

---

The following command creates the instance with VM_NAME(replace with suitable name) with the image as debain intead of Ubuntu as we used above. This command has an startup sript attached which runs the repo update -. installs apache2 -> and creates a badix index.html page which can be accessed by opening vm's public ip.

```
gcloud compute instances create VM_NAME \
  --image-project=debian-cloud \
  --image-family=debian-10 \
  --metadata=startup-script='#! /bin/bash
  apt update
  apt -y install apache2
  cat <<EOF > /var/www/html/index.html
  <html><body><p>Linux startup script added directly.</p></body></html>
  EOF'
```