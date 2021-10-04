### Creating a Virtual Machine

Create a google account

Go to [Google Cloud](https://cloud.google.com) and click **Get started for free**

Fill out all the details. You'll need to add your bank card. This is just to stop you doing this multiple times with different accounts. You won't be charged once the $300 is used up.

In the [Google Cloud Console](https://console.cloud.google.com), go to the **VM instances** page.

Select your project and click **Continue**.

Click **Create instance**.

Specify a **Name** for your VM.

*Optional*: Change the **Zone** for this VM. Compute Engine randomizes the list of zones within each region to encourage use across multiple zones. Each zone has different resoures available so one zone might not offer the CPU or GPU you're looking for. I usually just pick **us-central1 (Iowa)**.

Select a **Machine configuration** for your VM. [This](https://cloud.google.com/compute/docs/machine-types) explains the different machine types.

Allow **HTTPS** traffic

Click **Create** to create and start the VM



### Connecting to VS Code

Follow [these](https://cloud.google.com/sdk/docs/quickstart) instructions to install gcloud

The easiest for macOS is to use

```bash
$ brew install google-cloud-sdk
```

Initialise and login to Google Cloud

```bash
$ gcloud init
```

Create aliases to ssh into your VM's

```bash
$ gcloud compute config-ssh
```

Install the **Remote - SSH** extension in VS Code

Open the command pallet (⇧⌘P on MacOS) and select **Remote-SSH: Connect to Host...**

Your instance should show up. Click it to initialise and connect to the VM.

The file browser on the left should show your files on the VM.



### matplotlib forwarding to local machine browser

```python
matplotlib.use('WebAgg')
```



### Enable the VM to automatically shutdown

Still can't get this working...

First install wget

```bash
$ sudo apt-get install wget
```

Download the script

```bash
$ wget https://gist.githubusercontent.com/justinshenk/312b5e0ab7acc3b116f7bf3b6d888fa4/raw/59f021c2bf0388ba36e5a589dba52e233ee84964/idle-shutdown.sh
```

Place it somwehere in the VM

Select the VM in the console and enter edit mode

In the **Custom metadata** add

| Key            | Value                                   |
| -------------- | --------------------------------------- |
| startup-script | /<your home directory>/idle-shutdown.sh |

Install the package **bc** for CPU usage monitoring

```bash
$ sudo apt-get install bc
```

Reboot the instance