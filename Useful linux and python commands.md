# ssh login:-

* ssh username@hostname
* enter your password and you are logged in

# file transfer using scp

* Go to your directory in local system where the file is present
open terminal
* type:- scp filename username@hostname:~/
* for folder transfer :- scp -r foldername username@hostname:~/

# install package in any server using pip and proxy

* pip install --proxy http://username@hostname:port package_name --user
* eg. pip install --proxy http://mcs182012@10.10.78.62:3128 tensorflow-gpu --user

# XX is 22 for B.Tech., 62 for M.Tech. and 61 for Ph.D. Enter Port as 3128.

# install a package using tar file. 

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

# If you are using ANACONDA on hpc then you need to be careful while creating a virtual environment.

Here are the commands.
```
module load apps/anaconda/3
conda config --set auto_activate_base false
conda init
```

Then close the terminal and log in again.
```
module load apps/anaconda/3
conda create --prefix=/home/cse/mtech/mcs182012/myenv
module unload apps/anaconda/3
conda config --set env_prompt '({name}) '
conda activate /home/cse/mtech/mcs182012/myenv
```
Now you can install your packages using ```conda install pkgname``` command and it won't conflict with the existing packages.
