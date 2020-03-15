# HPC-IIT-Delhi

*HPC Details and Use of IIT Delhi*

Basic Things to Know about IIT Delhi HPC:
> There are several nodes or machines in the HPC cluster. Each GPU node/machine has 2 gpus. Each CPU node/machine has 1 cpu. So if you wish to request for 4 gpus then you have to request for 2 nodes. For the CPU case, we don't request for CPU directly, instead we request for cores. Each CPU node has some number of cores. You just request for some cores. Say 1 cpu node has 12 cores and you requested for 8 cores, then 8 cores out of 12 will be allocated to you. You can see the hardware details [here](http://supercomputing.iitd.ac.in/?info) and as per your requirement request accordingly. I will be describing some more details in below examples.

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


Now the main work comes. You need to specify the resources you need.

Type:-

qsub -IP cse -l select=1:ncpus=8:ngpus=1:mem=24G -l walltime=6:00:00

Wait till resources are allocated...

Use cd command move to your home directory. Eg:- cd ../../../home/cse/mtech/mcs182012

You can also use scratch to operate but all the files in scratch are auto deleted.

So you need to take care of this if you are working on scratch directory.

Home directory space - 30GB

Scratch directory space - 100GB

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
