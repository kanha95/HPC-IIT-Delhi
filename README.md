# HPC-IIT-Delhi

*HPC Details and Use of IIT Delhi*

Basic Things to Know about IIT Delhi HPC:
> There are several nodes or machines in the HPC cluster. Each GPU node/machine has 2 gpus. Each CPU node/machine has 1 cpu. So if you wish to request for 4 gpus then you have to request for 2 nodes. For the CPU case, we don't request for CPU directly, instead we request for cores. Each CPU node has some number of cores. You just request for some cores. Say 1 cpu node has 12 cores and you requested for 8 cores, then 8 cores out of 12 will be allocated to you. You can see the hardware details [here](http://supercomputing.iitd.ac.in/?info) and as per your requirement request accordingly. I will be describing some more details in below examples.

*Note*
* I assume you are familar with basic linux commands before diving into the world of HPC
* If not then do read about cd, mkdir, rm, cp, mv, man, chmod, chown, ifconfig, ls, cat, clear commands. These are basics and i feel it would be enough.

*Getting Login Access*
* Get HPC Access from here https://userm.iitd.ac.in/usermanage/hpc.html
* They would mail you once everything is ready.
* Note that i would be using Linux OS terminal for the purpose. If you are on Windows OS then you need to use putty & use command prompt accordingly
* Open Linux Terminal/Command Prompt
* Type:- ssh username@hpc.iitd.ac.in
* Enter your kerberos password
* Done you are now logged in
* Note that username is the short version of your entry number, like mine is mcs182012

*For First Time Login Only*
* These are some basic setting you need to do if you are logging into hpc for the first time
* Note that this is only for the users who login into hpc for the first time
```
   cp /home/apps/skeleton/.bashrc  $HOME/.bashrc 
   cp /home/apps/skeleton/.bash_profile $HOME/.bash_profile
   ln -s $SCRATCH $HOME/scratch
```

*How to transfer files?*
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
    scp -R foldername username@hpc.iitd.ac.in:~/
  ```
  The first command would transfer a file and 2nd one would transfer a folder. I would recoomend you to convert the folder into zip file and then transfer it using the first command. It would be faster as zip files are smaller in size. You can then later unzip the file on the HPC logged in terminal by typing:- unzip filename
* Now on the HPC logged in terminal type:- ```ls``` to check if the files are correctly transfered or not. 

*Now the main work comes. You need to specify the resources you need*

* There are 2 ways you can do so. One is interactive and the other is non-interactive mode.
* In the interactive mode you will visualise the entire process and in non-interactive mode you would submit the job and task to be done and it would be done automatically and a mail would be sent to you that the task is completed.
* If your code is ready then better go for non-interactive mode. If you want to make changes in the code then interactive mode is the only option you have.
* For Interactive Mode, On HPC logged In terminal type:-
```
qsub -I -P cse -l select=2:ncpus=8:ngpus=1:mem=24G -l walltime=6:00:00
```
  Wait till resources are allocated...
  In the above query I refers to 'Interactive mode', P refers to Project. So the above query means "We are requesting 2 nodes with 8 cpu cores each, 1 gpu each, 24GB RAM each for a total time of 6 Hours. 
  Note that you need to specify your branch or project name after -P like ee or cc or any other.
* Once the resources are allocated,
  Use cd command move to your home directory.
  ```
     cd ../../../home/cse/mtech/mcs182012
  ```
  You need to modify your branch name, degree and username accordingly in the above command.
  Now we are in the home directory. Home directory space is 30GB. Type:- ```ls``` command and you can see a directory name scratch. Type:- ```cd scratch``` to go into the sctrach. Scratch directory space is 200GB. You can operate from Home or Scratch directory. But note that files and folders inside scratch are auto deleted after some time. So you need to move them into home directory if you wish to save something.



Load python3.6 modules now...

module load compiler/python/3.6.0/ucs4/gnu/447 

module load pythonpackages/3.6.0/ucs4/gnu/447/pip/9.0.1/gnu

module load pythonpackages/3.6.0/ucs4/gnu/447/setuptools/34.3.2/gnu

module load pythonpackages/3.6.0/ucs4/gnu/447/wheel/0.30.0a0/gnu

module load apps/pythonpackages/3.6.0/tensorflow/1.9.0/gpu

module load apps/pythonpackages/3.6.0/keras/2.2.2/gpu

module load apps/pythonpackages/3.6.0/pytorch/0.4.1/gpu

module load apps/pythonpackages/3.6.0/tensorflow/1.9.0/gpu

module load apps/pythonpackages/3.6.0/torchvision/0.2.1/gpu


Download IITD Certificate from here - http://www.cc.iitd.ernet.in/certs/CCIITD-CA.crt

Transfer to filezilla using drag and drop again.

Now use the command.

export SSL_CERT_FILE=$HOME/CCIITD-CA.crt 

lynx https://proxy62.iitd.ernet.in/cgi-bin/proxy.cgi

Login using id and password.

Now set the http and https proxy....

export http_proxy=10.10.78.62:3128

export https_proxy=10.10.78.62:3128

install python3 libraries now...

pip install scipy matplotlib scikit-learn scikit-image numpy pandas --user

Alternatively HPC has a lot of libraries. Just use the command :- module avail

to list all of them.

Copy the module text you want and type :- module load YOUR-MODULE-LINK

to load the module.

Now run using python command.

python test.py

You can use the command :- qstat -u $USER

to see the status of your resources.

You can then delete any resources using the command:- qdel JOBID

JOBID is the number obtained after type qstat -u $USER

If you stopped your program in the middle of execution, then it will give CUDA OUT OF MEMORY error next time if you try to run program on gpu from the same login node.

So you need to flush the gpu.

Type:-

nvidia-smi

kill -9 PID
