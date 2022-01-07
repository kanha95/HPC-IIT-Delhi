# HPC-IIT-Delhi

*HPC Details and Use of IIT Delhi*

Basic Things to Know about IIT Delhi HPC:
> There are several nodes or machines in the HPC cluster. Each GPU node/machine has 2 gpus. Each CPU node/machine has 1 cpu. So if you wish to request for 4 gpus then you have to request for 2 nodes. For the CPU case, we don't request for CPU directly, instead we request for cores. Each CPU node has some number of cores. You just request for some cores. Say 1 cpu node has 12 cores and you requested for 8 cores, then 8 cores out of 12 will be allocated to you. You can see the hardware details [here](http://supercomputing.iitd.ac.in/?info) and as per your requirement request accordingly. I will be describing some more details in below examples.

## NOTE
* I assume you are familar with basic linux commands before diving into the world of HPC
* If not then do read about cd, mkdir, rm, cp, mv, man, vim, vi, chmod, chown, ifconfig, ls, cat, clear commands. These are basics and i feel it would be enough.

## TERMINAL DETAILS
* The terminal that you get first when you login is your home terminal
* The terminal that you get after resources have been alloted is your resource alloted terminal
* All the files and folders in home terminal are accessible by the resource alloted terminal
* The environement of home terminal is not same as resource alloted terminal, you need to specify the environement exclusively to use for resource alloted terminal. You can specify the environment by using conda as described below or use the default given modules of HPC by loading them.
* By environment, i mean the packages, libraries and settings to use during execution of your scripts

## GETTING LOGIN ACCESS AND LOGGING IN
* Get HPC Access from here https://userm.iitd.ac.in/usermanage/hpc.html
* They would mail you once everything is ready.
* Note that i would be using Linux OS terminal for the purpose. If you are on Windows OS then you need to use putty & use command prompt accordingly
* Open Linux Terminal/Command Prompt
* Type:- ssh username@hpc.iitd.ac.in
* Enter your kerberos password
* Done you are now logged in
* Note that username is the short version of your entry number, like mine is mcs182012

***FOR FIRST TIME LOGIN ONLY***
* These are some basic setting you need to do if you are logging into hpc for the first time
* Note that this is only for the users who login into hpc for the first time
```
   cp /home/apps/skeleton/.bashrc  $HOME/.bashrc 
   cp /home/apps/skeleton/.bash_profile $HOME/.bash_profile
   ln -s $SCRATCH $HOME/scratch
   chmod og-rx $SCRATCH
   chmod og-rx $HOME
```

## HOW TO TRANSFER FILES?
* If you wish to transfer via a graphical user interface then Download filezilla. 
  Login into filezilla using:-
```
    Hostname = hpc.iitd.ac.in
    Username = mcs182012
    Port = 22
    Password = your_kerberos_password
```
  Now Transfer your files using filezilla drag and drop.
* The 2nd way you can transfer is using scp command. This is what i would recommend as it is bit faster and just so easy!!!
  Just go the directory where your file/folder is stored that you wish to transfer. Right Click -> Open Terminal and then
  ```
    scp filename username@hpc.iitd.ac.in:~/
    or
    scp -r foldername username@hpc.iitd.ac.in:~/
  ```
  The first command would transfer a file and 2nd one would transfer a folder. I would recommend you to convert the folder into zip file and then transfer it using the first command. It would be faster as zip files are smaller in size. You can then later unzip the file on the HPC logged in terminal by typing:- unzip filename
* Now on the HPC logged in terminal type:- ```ls``` to check if the files are correctly transfered or not. 

*Now the main work comes. You need to specify the resources you need*

### Note that you can run your python files or scripts only after resources are alloted to you. Don't run those files on the login terminal. It will give you warning mail from HPC and terminate your HPC access

* There are 2 ways you can do so. One is interactive and the other is non-interactive mode.
* In the interactive mode you will visualise the entire process and in non-interactive mode you would submit the job and task to be done and it would be done automatically and a mail would be sent to you that the task is completed.
* If your code is ready then better go for non-interactive mode. If you want to make changes in the code then interactive mode is the only option you have.
* **For Interactive Mode**, On HPC logged In terminal type:-
```
qsub -I -P cse -l select=2:ncpus=8:ngpus=1:mem=24G -l walltime=6:00:00
or
qsub -I -P cse -l select=2:ncpus=8:ngpus=1:mem=24G:centos=skylake -l walltime=6:00:00
```
  Wait till resources are allocated...
   In the above first query, I refers to 'Interactive mode', P refers to Project. So the above query means "We are requesting 2 nodes with 8 cpu cores each, 1 gpu each, 24GB RAM each, haswell phase 1 nodes are to be selected, for a total time of 6 Hours.
  
  In the above second query, I refers to 'Interactive mode', P refers to Project. So the above query means "We are requesting 2 nodes with 8 cpu cores each, 1 gpu each, 24GB RAM each, skylake phase 2 nodes are to be selected, for a total time of 6 Hours. 
  
  Note that phase 2 nodes are powerful and faster ones. Generally they don't give these nodes to all. You need to pass advance HPC test conducted at college to avail those. But sometimes they are available to use without giving any test. You can see the Phase 1 and 2 node details [here](http://supercomputing.iitd.ac.in/?info)
  
  Also Note that you need to specify your branch or project name after -P like ee or cc or any other.
* Once the resources are allocated,
  Use cd command move to your home directory.
  ```
     cd ~/
  ```
  At this point you should be getting a display like ``` username@login01 ```.
  Now we are in the home directory. Home directory space is 30GB. Type:- ```ls``` command and you can see a directory name scratch. Type:- ```cd scratch``` to go into the scratch. Scratch directory space is 200TB. You can operate from Home or Scratch directory. But note that files and folders inside scratch are auto deleted after some time. So you need to move them into home directory if you wish to save something.
* **For non-interactive mode**, I have provided a file with name ```pbsbatch.sh```, transfer it using the above mentioned techniques, then you just need to open the file using the command ```vi pbsbatch.sh```, then start editing by typing ```i```, then specifiy your requirements accordingly, then save it using the command ```ESCAPEKEY:wq```. Note that you need to press the ESCAPEKEY and then release it followed by :wq. Now type ```qsub pbsbatch.sh```, if everything you have specified is correct your job will be submitted and once done you will get a mail regarding the same. 2 files will also be created in the home directory, use ```vi filename``` to open those files. One of them is the error log file and the other is output log file. I have commented the ```pbsbatch.sh``` file to help you understand the things inside the file. They are mostly similar to the commands that we would be working in interactive mode.

## CHECK DISK QUOTA
```
   lfs quota -hu $USER /home
   lfs quota -hu $USER /scratch
```
You can check the disk quota using the above 2 commands. Normally for home it is 30GB and for scratch it is 200TB. You can mail to *hpchelp@iitd.ac.in* to increase home disk quota if you need more space.

## LOADING MODULES

* You can see the list of all preloaded modules by typing on HPC logged in terminal ```module avail```
* Now select the package you wish to use and load them typing ```module load packagename```
* Example:-
```
  module load compiler/python/3.6.0/ucs4/gnu/447 
  module load pythonpackages/3.6.0/ucs4/gnu/447/pip/9.0.1/gnu
  module load pythonpackages/3.6.0/ucs4/gnu/447/setuptools/34.3.2/gnu
  module load pythonpackages/3.6.0/ucs4/gnu/447/wheel/0.30.0a0/gnu
  module load apps/pythonpackages/3.6.0/tensorflow/1.9.0/gpu
  module load apps/pythonpackages/3.6.0/keras/2.2.2/gpu
  module load apps/pythonpackages/3.6.0/pytorch/0.4.1/gpu
  module load apps/pythonpackages/3.6.0/tensorflow/1.9.0/gpu
  module load apps/pythonpackages/3.6.0/torchvision/0.2.1/gpu
```
* Note that you can load modules in home terminal. But it won't be used once the resources are alloted to you. So after resources are alloted to you, you need to again load the modules you wish to use. If you don't wish to use these default modules and wish to install your own packages then better use conda environment as i have listed below. Using python venv has some issues, so i would suggest better to go for conda environment. IITD HPC supports conda environment well.

## INSTALLING PACKAGES FROM THE INTERNET OR WEB

* This procedure will give internet connectivity to your current terminal. You can use the below techniques either in home terminal or after request alloted terminal.
* Download IITD Certificate from here - http://www.cc.iitd.ernet.in/certs/CCIITD-CA.crt
* Transfer the file using the techniques mentioned above
* Also transfer the 2 files i have provided ```proxyAuth.txt``` and ```iitdproxy.py```.
* On the HPC logged In Terminal type ```export SSL_CERT_FILE=$HOME/CCIITD-CA.crt```. This would add the IIT Delhi certificate file name to the SSL_CERT_FILE variable. You can check the variable value by typing ```echo $SSL_CERT_FILE```. 
* Now in the provided file ```proxyAuth.txt```, open it using the command ```vi proxyAuth.txt```, start editing by typing ```i```, then update your username & branch and then save it using ```ESCAPEKEY:wq```. Note that you need to press the ESCAPEKEY and then release it followed by :wq 
* Then run the command ```python2 iitdproxy.py proxyAuth.txt```. It will ask for password type in your kerberos password
* It will show login success, Now leave the terminal as it is.
* DONOT CLOSE THIS TERMINAL. MINIMISE IT.
* Open a New Terminal, login into hpc using the ssh command as mentioned in the *Getting Login Access and LOGGING IN* section.
* Now we need to export the http, ftp and https proxy, Run the command
  ```
    export http_proxy=http://10.10.78.XX:3128
    export ftp_proxy=10.10.78.XX:3128
    export https_proxy=http://10.10.78.XX:3128
  ```
  Note that ```XX``` is 22 for B.Tech, 62 for M.Tech and 61 for Ph.D.
* Now you can install the packages from web using pip commands,
  E.g. ```pip install scipy matplotlib scikit-learn scikit-image numpy pandas --user```
* ADDITIONAL NOTE:-
  On HPC website they have mentioned a different way using ```lynx https://proxyXX.iitd.ernet.in/cgi-bin/proxy.cgi``` command but please don't use it as it breaks the seesion after 5-10 minutes and then you simply can't use the internet. If you try to log in then it will show error saying already logged in from some other system.

## INSTALL A PACKAGE USING TAR/ZIP FILE

* Go to pypi.org. Search package you wish for.
* In download files section download the relevant tar or zip file.
* Do scp to transfer the file from local system to the server as mentioned above in scp section.
* unzip the zip file or untar the tar file using the command:-
```
  unzip filename
  or
  tar -xzvf filename
  cd foldername
  python3 -m setup.py install --user
```

## If you are using ANACONDA on hpc then you need to be careful while creating a virtual environment.

Here are the commands. Use the below commands in home terminal without requesting for any resoruces.
```
module load apps/anaconda/3
conda config --set auto_activate_base false
conda init
```

Then **close all the terminals** and log in again. Now provide internet connectivity to current terminal. Do read the section INSTALLING PACKAGES FROM THE INTERNET OR WEB above to give connectivity to this terminal. Else you will get https error. Use the below commands in home terminal without requesting for any resoruces.

```
module load apps/anaconda/3
conda create --prefix=~/myenv
module unload apps/anaconda/3
conda config --set env_prompt '({name}) '
conda activate ~/myenv or source activate ~/myenv
```
Now you can install your packages using ```conda install pkgname``` command and it won't conflict with the existing packages. After installing all the packages when your environment is ready deactivate the environment using ```conda deactivate ~/myenv```. 

### NOTE:- Use ```source activate ~/myenv``` if you are using non-interactive mode.

You can create the environment in the home login terminal without requesting for any resources. Then once the request is made and resources are allocated, you just need to activate the environment, i.e.

```
    module load apps/anaconda/3
    source activate ~/myenv
    module unload apps/anaconda/3
```

We are unloading the module anaconda3 because we want out environment packages to be used instead of base conda packages. Now all the packages that you have installed in your environment will be used.

## PROJECT FUNDS CHECK
* Note that this section is only for people who have funded projects on HPC
* Funded projects help you gain access to high priority queue where getting resources is easier and quicker
  You can then just use ```-q high``` to specifiy that you are requesting for high priority queue instead of the regular standard queue like this ```qsub -q high pbsbatch.sh``` or ```qsub -I -P cse -q high -l select=2:ncpus=8:ngpus=1:mem=24G -l walltime=6:00:00```
* You can fund your projects too, just mail to *hpchelp@iitd.ac.in* for all the details
* For charges please visit [this](http://supercomputing.iitd.ac.in/?charge) link
* You can check the left balance of your funds by typing these commands:-
  ```
    amgr login
    amgr ls project
    EnterKerberosPassword
    amgr checkbalance project -n cse
    amgr checkbalance project -n ml.cse -p global
   ```
   
## SOME OTHER USEFUL COMMANDS
* To see the status of your allocated resources, You can use the command ```qstat -u $USER```
* You can then delete any allocated resources using the command ```qdel JOBID```
  JOBID is the number obtained after typing ```qstat -u $USER```. It is a 5 digit number generally before the dot character.
* If you ever stop your running gpu utilising program in the middle of execution, then it will give CUDA OUT OF MEMORY error next time if you try to run program on gpu from the same login node.
  So you need to flush the gpu. Type:- ```nvidia-smi```
  Note down the PID. Then kill the PID by typing ```kill -9 PID```
* You can load anaconda typing ```module load apps/anaconda/3``` and then install the packages using ```conda install packagename``` command. You need to given internet access to the login node as i mentioned above.
* You can see the nodes allocated by typing ```cat $PBS_NODEFILE``` after the allocation is done
* For passwordless login see this [link](http://supercomputing.iitd.ac.in/?FAQ#sshkeys)
* If you wish to use HPC from outside IITD Campus then you need to write a mail to *updaters@cse.iitd.ac.in* with subject *Getting SRI access*. Mention the reason you want to use HPC outside IITD, CC your faculty incharge. Once approved you can use the following commands to login to HPC:-
```
   ssh username@sri.cse.iitd.ac.in
   EnterKerberosPassword
   ssh username@hpc.iitd.ac.in
   EnterKerberosPassword
 ```
 * You can also get access to hpc from outside campus using VPN services of CSC. Please visit [this](http://www.cc.iitd.ernet.in/CSC/index.php?option=com_content&view=article&id=104&Itemid=135) website for more details on it. Once you get the VPN access, On WINDOWS download and install OPENVPN. Then download all the certificates and all provided to you. They will mail you all the certificates and files. Transfer it inside /etc/openvpn/client folder. Click on connect. It will be connected. Now use putty software on WINDOWS to ssh into HPC. The above steps works well on WINDOWS. Refer this [link](https://github.com/kanha95/VPN-IIT-Delhi) for VPN setup on linux.
 
 * VPN connections can be sensitive to incorrect/low MTU set within your network, or on networks between you and your server. It is important that the correct MTU is set, to ensure fast and error-free VPN performance. 
 Please visit this [link](https://www.sonassi.com/help/troubleshooting/setting-correct-mtu-for-openvpn) for details.
 
 * For any help regarding HPC, mail to *hpchelp@iitd.ac.in*
