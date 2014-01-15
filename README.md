Vagrant Demo Environment
========================

A repository which a series of steps I did for a vagrant demo at work (Connecta).

Step 0
------

Make sure you have the following installed and configured in your path:
 * putty (full package)
 * tortoisegit (and msysgit)
 * vagrant

1. Open a command windows and change to an empty directory:

   ```
   cd c:\
   mkdir vagrant-lab
   ```

2. Clone the git repository and cd into it

   ```
   git clone https://github.com/mickem/vagrant-demo.git/
   cd vagrant-demo
   ```

Step 1
------

1. checkout tag step 1

   ```
   git checkout step-1
  ```

2. Start your machine with vagrant

   You start virtual machines with the vagrant up command.

   ```
   vagrant up
   Bringing machine 'default' up with 'virtualbox' provider...
   [default] Importing base box 'vagrant-demo'...
   [default] Matching MAC address for NAT networking...
   [default] Setting the name of the VM...
   [default] Clearing any previously set forwarded ports...
   [default] Creating shared folders metadata...
   [default] Clearing any previously set network interfaces...
   [default] Preparing network interfaces based on configuration...
   [default] Forwarding ports...
   [default] -- 22 => 2222 (adapter 1)
   [default] Running 'pre-boot' VM customizations...
   [default] Booting VM...
   [default] Waiting for machine to boot. This may take a few minutes...
   [default] Machine booted and ready!
   [default] Setting hostname...
   [default] Mounting shared folders...
   [default] -- /vagrant
   ```

3. Access machine with ssh:

   On windows vagrant does not nativly support ssh so running vagrant ssh will prompt you with solutions to access your machine.
   
   ```
   vagrant ssh
   `ssh` executable not found in any directories in the %PATH% variable. Is an
   SSH client installed? Try installing Cygwin, MinGW or Git, all of which
   contain an SSH client. Or use the PuTTY SSH client with the following
   authentication information shown below:
   
   Host: 127.0.0.1
   Port: 2222
   Username: vagrant
   Private key: C:/Users/michael.medin/.vagrant.d/insecure_private_key
   ```

4. Convert the key

   Open puttygen and import the vagrant private key: C:\Users\USERID\.vagrant.d\insecure_private_key
   Then save this as C:\Users\USERID\.vagrant.d\insecure_private_key.ppk
   
5. Configure putty

   One way to handle this is to create a generic vagrant putty profile where you configure the following.
   
   |  Key                                  | Value                                               |
   |---------------------------------------|-----------------------------------------------------|
   | Session > Host name:                  | localhost                                           |
   | Session > Port:                       | 22                                                  |
   | Session > Name:                       | vagrant                                             |
   | Connection > Data > Login             | vagrant                                             |
   | Connection > SSH > Auth > Private Key | C:\Users\USERID\.vagrant.d\insecure_private_key.ppk |

6. Use putty to ssh into the machine.

   We can now use the profile and manually override the port on the putty command line to ssh into our machine.
   
   ```
   putty -load vagrant -P 2222
   
Step 2
------

1. Add a shared folder to the virtual machine

   Make the folder we want to share:
   ```
   mkdir scripts
   ```
   
   Then add the following to the Vagrantfile to share the local folder scripts with the virtual machine as /home/vagrant/scripts.
   ```
     config.vm.synced_folder "scripts", "/home/vagrant/scripts"
   ```

2. Add a provisioning shell script

   Add a provisioning shell script to the Vagrantfile
   ```
     config.vm.provision "shell", inline: "su - vagrant -c ./scripts/test.sh"
   ```
   
   Then create the following file in a folder called scripts:
   ```
   #/bin/sh
   echo "Hello World" >> /home/vagrant/hello.txt
   ```

3. Verify your solution with the answer:
   
   You can always use git to validate the solution:
   ```
   git diff step-2
   ```

4. (re)Provision your new configuration.

   While vagrant up will also run a provisioning it requires the macine to be off.
   If you just want to re-provision your config you instead run:
   ```
   vagrant provision
   ...
   -su: ./scripts/test.sh: No such file or directory
   ```
   
   The reason this failes is that adding shared folders requires a reload of the vagrant box. To do so run the following command:
   ```
   vagrant reload
   [default] Attempting graceful shutdown of VM...
   [default] Clearing any previously set forwarded ports...
   [default] Creating shared folders metadata...
   [default] Clearing any previously set network interfaces...
   [default] Preparing network interfaces based on configuration...
   [default] Forwarding ports...
   [default] -- 22 => 2222 (adapter 1)
   [default] Running 'pre-boot' VM customizations...
   [default] Booting VM...
   [default] Waiting for machine to boot. This may take a few minutes...
   [default] Machine booted and ready!
   [default] Setting hostname...
   [default] Mounting shared folders...
   [default] -- /vagrant
   [default] -- /home/vagrant/scripts
   ```
   This will re-start the virtual machine. After this rerun the provisioning step which should now work.
   ```
   vagrant provision
   [default] Running provisioner: shell...
   [default] Running: inline script
   stdin: is not a tty
   ```

5. Verify that the script has executed by checking that the hello.txt file was created in the vagrant home directoy.
   
   ```
   putty -load vagrant -P 2222
   ```
   Then in the putty window run:
   ```
   vagrant@vagrant:~$ more hello.txt
   Hello World
   exit
   ```

Step 3
------

TODO

Step 4
------

TODO
