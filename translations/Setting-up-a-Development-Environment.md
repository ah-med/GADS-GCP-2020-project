# App Dev: Setting up a Development Environment v1.1

## Objectives

In this lab, you will learn how to perform the following tasks:

- Provision a Google Compute Engine instance.

- Connect to the instance using SSH.

- Install software on the instance.

- Verify the software installation.

## Steps

### 1. Create a Compute Engine Virtual Machine Instance

- The following command will create a compute engine virtual machine instance with name as *dev-instance* in the zone `us-central1-a` of `us-central1` region, set Identity and api access to allow full access to all Cloud APIs and set Firewall to Allow HTTP traffic. Leaving the remaining settings as their defaults

```shell
gcloud compute instances create dev-instance --zone=us-central1-a --scopes=https://www.googleapis.com/auth/cloud-platform --tags=http-server
```

### 2. Install software on the VM instance

- ssh into dev-instance

```shell
gcloud compute ssh --zone "us-central1-a" "dev-instance" --project "qwiklabs-gcp-00-c7fcb2a12fb4"
```

- In the SSH session, update the Debian package list

```shell
sudo apt-get update
```

- Install Git by execute the following command:

```shell
sudo apt-get install git
```

- If prompted, press Enter.

- Download the Node.js setup script, execute the following command:

```shell
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
```

- Install npm and Node.js, execute the following command:

```shell
sudo apt install nodejs
```

```shell
sudo apt install npm
```


- Again, if prompted press Enter

### 3. Configure the VM to Run Application Software

- Check the version of Node.js and npm, execute the following command:

```shell
node -v
```

```shell
npm -v
```

- Clone the class repository, execute the following command:

```shell
git clone https://github.com/GoogleCloudPlatform/training-data-analyst
```

- Change the working directory, execute the following command:

```shell
cd ~/training-data-analyst/courses/developingapps/nodejs/devenv/
```

- Run a simple web server, execute the following command:

```shell
sudo node server/app.js
```

Return to the Cloud Console VM instances list, and click on the External IP address for the dev-instance.

You should see a Hello GCP dev! message from Node.js.

Return to the SSH window, and stop the application by pressing `Ctrl+C`.

- Install the Node.js library for Compute Engine, execute the following command:

```shell
npm install
```

- Run a simple Node.js application that lists Compute Engine instances, execute the following command:

```shell
node list-gce-instances.js
```

Many details about your machine should appear in the terminal window.

```Warning: If you try to do this on your own machine, it will not work if no credentials have been set up to access GCP on your machine.```
