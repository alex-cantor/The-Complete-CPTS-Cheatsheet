# Building Your Environment
It is extremely important to have an environment that you are comfortable working in. As I have gone through the CPTS pathway, I have built out my machine to include all of the tools that I may need on the big "game day." This way, you will not have to download these tools later.

## Preface

I have found Kali Linux to be the best tool for me. You may like something different, but this MD will go into depth on setting up your Kali environment

## Getting Started

**Prerequisits**: Make sure that you already have VMware installed. I would recommend VMWare Workstation Pro. My machine runs on `VMware workstation 17 Pro, version 17.5.2`

We will start by downloading our VM. I want this step to be as seemless as possible, and so I am going to download a pre-built virtual machine
1. Go to `https://www.kali.org/get-kali/#kali-virtual-machines`
2. Select the download button for `VMware`
3. Extract the completed download
4. Open `VMware`
5. Select `Open a Virtual Machine`
6. Navigate to the directory of the extracted file, and open the folder. This will reveal one file, which is what you will select
7. Let's allocate a few more resources to the machine. **Note**: Make sure you check your specs (`Task Manager` > `CPU` or `Memory`)
   a. Edit virtual machine settings
   b. My PC has 16GB of RAM, so I allocated 10 GB to the VM
   c. I allocated 8 processors
8. Click `Play this Virtual Machine`
9. Enter in the default creds: `kali:kali`

We have got Kali Linux all set up! Now, let's install some more tools...

1. Install all "Kali" tools: `sudo apt update && sudo apt install kali-linux-everything`
2. Install CrackMapExec: `sudo apt install crackmapexec`
3. Install BloodHound: `sudo apt install bloodhound`
4. Install Impacket: `sudo apt install impacket-scripts`
5. Install SecLists: `sudo apt install seclists`
6. Install Empire:
   ```bash
   git clone https://github.com/BC-SECURITY/Empire.git
   cd Empire
   ./setup/install.sh
   ```
7. Install Hashcat: `sudo apt install hashcat`
8. Install Sublist3r:
    ```bash
    git clone https://github.com/aboul3la/Sublist3r.git
    cd Sublist3r
    pip install -r requirements.txt
    ```
