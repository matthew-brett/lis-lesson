# Setup

Followed default setup for [Google Cloud](https://tljh.jupyter.org/en/latest/install/google.html).

Having started the VM, I noted the IP, then reserved it, according to [this
doc
section](https://cloud.google.com/compute/docs/ip-addresses/reserve-static-external-ip-address#promote_ephemeral_ip)

```
gcloud compute addresses create lishub --addresses 35.230.133.113 --region europe-west2
```

This might create the machine

```
gcloud compute instances create lis-jupyterhub --project=uob-testing --zone=europe-west2-b --machine-type=e2-highmem-2 --network-interface=address=35.230.133.113,network-tier=PREMIUM,subnet=default --metadata=startup-script=\#\!/bin/bash$'\n'curl\ -L\ https://tljh.jupyter.org/bootstrap.py\ \\$'\n'\ \ \|\ sudo\ python3\ -\ \\$'\n'\ \ \ \ --admin\ mb312 --maintenance-policy=MIGRATE --no-service-account --no-scopes --tags=http-server,https-server --create-disk=auto-delete=yes,boot=yes,device-name=lis-jupyterhub,image=projects/ubuntu-os-cloud/global/images/ubuntu-1804-bionic-v20220419,mode=rw,size=10,type=projects/uob-testing/zones/europe-west2-b/diskTypes/pd-balanced --no-shielded-secure-boot --shielded-vtpm --shielded-integrity-monitoring --reservation-affinity=any
```

Other setup is adding LetsEncrypt, enabling anyone to log in:

```
sudo tljh-config set https.enabled true
sudo tljh-config set https.letsencrypt.email matthew.brett@gmail.com
sudo tljh-config add-item https.letsencrypt.domains lishub.odsti.org
```

```
sudo tljh-config set auth.FirstUseAuthenticator.create_users true
sudo tljh-config reload
```

`sudo tljh-config show` gives:

```
users:
  admin:
  - my_user
https:
  enabled: true
  letsencrypt:
    email: matthew.brett@gmail.com
    domains:
    - lishub.odsti.org
auth:
  FirstUseAuthenticator:
    create_users: true
```

Pip installing packages:

```
sudo -E pip install pandas matplotlib folium jupytext
```
