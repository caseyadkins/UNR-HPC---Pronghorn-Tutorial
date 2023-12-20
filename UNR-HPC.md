# UNR HPC - Pronghorn Tutorial

Authored by Casey Adkins and Ellen McMullen

### Overview

This workflow takes you through the steps of gaining access and to the University of Nevada specific research resources - the HPC Pronghorn and cloud storage via Rosalind. Followed with basic usage and software installation.

**Outline**

1. [Setting Up Accounts](#accounts)  
   A) UNR VPN  
   B) Rosalind  
   C) Pronghorn  
2. [Transferring Data](#transferring)  
   A) FileZilla  
   B) `scp` and `rsync`  
3. [Installing Packages](#installing)  
   A) `conda`  
4. [Conclusions](#Conclusions)
## 1. Setting Up Accounts<a name="accounts"></a>

To follow this workflow, you will need access to Pronghorn (UNR HPC). Additionally, as a graduate student you have access to 1T of the cloud data storage, Roslind, a UNR storage server. This server is very useful to backing up your data. If you are not working on the UNR campus, you will also need access to the UNR VPN to access Rosalind on your local computer.

### A) UNR VPN

#### Requesting Access

First you will need to go to the [service request form](https://unr.teamdynamix.com/TDClient/2684/Portal/Requests/ServiceDet?ID=47394). Login with your netID and password. Then click *request service*, and fill out the form with your name, computer host name, and justification for access.

Your computer host name can be found:

* **On Mac** by going to system settings --> General --> Sharing. The host name is listed at the bottom under *Local Hostname*.
* **On Windows** search and open the Command Prompt by typing command prompt or cmp into the Windows search bar --> within your cmp type `hostname` --> your local host name will be printed

Your justification for access can be something very simple like: "I need to access Rosalind and Pronghorn from off campus".

When you submit the form, you will get an email with your service request ID number, then a second email when you have been granted access.

#### Connecting to the VPN

Once you have been granted access, you need to download the VPN application. Go to [https://vpn.unr.edu](https://vpn.unr.edu). Login with your netID and password. You may also be asked to use a MFA method as well. Download the appropriate GlobalProtect client installer for your operating system.

During the installation, select both *GlobalProtect* and *GlobalProtect System extensions*. When the installation is complete, a welcome window will pop up. Click on *get started* on this window, then enter vpn.unr.edu when prompted for a portal. This may redirect you to enter your netID and password again, and then you will be connected!

**Note:** The Mac version of Global Protect does not seem to function properly on some Apple silicon (rather than Intel) chips. If you have a newer Mac, keep the installer on your computer. You may have to uninstall and reinstall Global Protect each time you use it, as it might only allow you to login upon first use.

### B) Rosalind

Rosalind is not required for this workflow if your data is stored locally or on Pronghorn. It is free cloud-storage available to UNR students and/or lab groups - making it a great location for backing up files and storing data and results.

#### Requesting Access

First you will need to to go to the [service request form](https://unr.teamdynamix.com/TDClient/2684/Portal/Requests/ServiceDet?ID=50652). Login with your netID and password. Then click *request service*, and fill out the form with your name, the type of storage (individual) and use type (primary). Fill out your PI's contact information so that they can approve your access.

When you submit the form, you will get an email with your service request ID number, then a second email when you have been granted access.

#### Connecting to Rosalind

**On Mac:** Open the Finder app. On the top menu bar click *Go* --> *Connect to Server*. Enter the address you were given in your access email, smb://rosalind.unr.edu/**YourPath**. Then login using your netID and password when prompted.

**On Windows or Linux:** See this [tutorial](https://www.unr.edu/cyberinfrastructure/research-data-storage-and-transfer/rosalind-onboarding).

### C) Pronghorn

#### Requesting Access

First you will need to request an account by going to the [HPC accounts page](https://www.unr.edu/cyberinfrastructure/high-performance-computing/accounts). Click *Apply for a Pronghorn Account*, then login with your netID and password. Fill out your contact information and answer the questions about your research.

When you submit the form, you will get an email with a reference number, then a second email when your account has been created. Your account will be created on the Monday following the request. When your account is created you will be added to a Pronghorn support slack channel as well. Within this channel you can find solutions to questions you may have or ask new questions of the Pronghorn staff and other users.

#### Connecting to Pronghorn

**On Mac or Linux:** Open Terminal and type the command `ssh netID@pronghorn.rc.unr.edu` with your netID filled in. You will be asked to enter your netID password.

**On Windows:** Install [PuTTY](https://www.putty.org/) - a free terminal emulator for Windows. Once installed, open the interface "PuTTY Configuration" and within the Session category fill out the host name and port. Host name: `pronghorn.rc.unr.edu`, Port: `22`, Connection Type: `SSH`. Click *Open*, this will open a terminal and you will be asked to enter your netID password.

**Note:** The password characters will not be displayed as you type them.

Once you have logged in, you can use the typical unix commands such as `pwd` to view the path of your home directory, */data/gpfs/home/****netID***.

When you are done, you can logout with the command `exit`.

Additional [Pronghorn documentation](https://github.com/UNR-HPC/pronghorn/wiki) can be found here.

## 2. Transfering Data<a name="transferring"></a>

### Using FileZilla

Download the appropriate version of [FileZilla](https://filezilla-project.org/download.php?show_all=1) for your operating system. Open the application.

On the top menu bar select *File* --> *Site Manager*. In the site manager window, select *New Site* and name it Pronghorn. Under the General Settings Tab, set Protocol to SFTP, set Host to pronghorn.rc.unr.edu, and set Port to 22. Next, set Logon Type to Normal, enter your netID and password, and click Connect.

You can use the GUI to transfer files from your local computer to Pronghorn. When you are done, select *Server* --> *Disconnect*.

### Using `scp` or `rsync`

The basic syntax of `scp` is `scp path/to/source/file path/to/destination/folder`. For example, if you want to copy something from your local computer to a Pronghorn directory, the destination path would be something like *netID@pronghorn.rc.unr.edu:data/gpfs/home/netID/file.txt*.

**Note:** `scp` will always overwrite existing files.

`rysnc` is similar to `scp` but can be set to only transfer files that differ between the locations. You can use the same syntax to specify a path on pronghorn.

## 3. Installing Packages<a name="installing"></a>

### Installing `conda`

1. Login to pronghorn with `ssh netID@pronghorn.rc.unr.edu` or via PuTTY.
2. Go to the [linux installation page](https://docs.anaconda.com/free/anaconda/install/linux/) and copy the prompt for Linux x86; it should be something like `curl -O https://repo.anaconda.com/archive/Anaconda3-2023.09-0-Linux-x86_64.sh`.
3. `cd` to the directory you want to install conda in (your home directory should be fine - /data/gpfs/home/netID)
4. Run the `curl` command.
5. When the download is complete, run `bash Anaconda3-2023.09-0-Linux-x86_64.sh` (filled in with the up-to-date version you downloaded).
6. Press enter to view the License, and press and hold enter to scroll through the License, or press `q` to skip to the agreement. Enter `yes` to agree. Press enter again to begin the installation.
7. When the installation finishes, enter `yes` again to update. your shell profile to use conda.
8. Run the command `source ~/.bashrc` to refresh the shell settings. You should now see (base) before your prompts, indicating that you are in the base conda environment. Alternatively, you can exit Pronghorn and re-login.
9. Test that conda is working correctly by running `conda list`. You should see a list of installed packages.
10. After you have confirmed that conda is working correctly, you can delete the installer with `rm Anaconda3-2023.09-0-Linux-x86_64.sh`

**Note:** On windows, after installing conda, if (base) or (your\_environment\_name) does not show up in the prompt, conda prompts may not work. Try running `bash` to get the environment name to show up in the prompt and test that conda is working with `conda list`. This applies for all future conda prompts; if they are not working, try to running `bash` first.

### Creating your conda environment

You may want to create a conda environment for specific projects and software. By orgainzing software into environments, you can work with different software versions. This can be important because some software require other software (e.g., python) to be a specific or older version than others.

```
conda create -n myenvname
conda activate myenvname
```

### 4. Conclusions<a name="Conclusions"></a>

You are now ready to start working on the UNR HPC - Pronghorn and some basic commands you may need.
