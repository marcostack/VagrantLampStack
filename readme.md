## Vagrant LAMP stack provisioning script

Vagrant creates a virtual machine using Virtualbox.  You can then use this machine to simulate a server on your computer.

### Why you should use this:
- Clean server installation for each project.  No conflicting dependencies.
- Private network.  Your vagrant box will have its own local IP address to access from your browser.
- Synced/Shared folders.  The provisioning script creates a shared folder between your host and your vagrant box.  The base project folder will be mounted on your vagrant box at `/vagrant` and also symlinked to `/var/www/html/project_folder`.  Set project_folder name in bootstrap.sh

### The included files:
- Vagrantfile
- bootstrap.sh

### Dependencies:
1. Vagrant https://www.vagrantup.com/downloads.html
2. VirtualBox https://www.virtualbox.org/wiki/Downloads


### Quickstart:
1. Install the dependencies & Vagrant https://www.vagrantup.com/docs/installation/
2. Place the included files inside the root of your project folder.  Your project folder should look like this:

        Project folder structure:
        .
        ├── Project/
        |      Vagrantfile
        |      bootstrap.sh
        |  ├── Project/
        |  |  ├── wp-admin/
        |  |  ├── ...
        |  |  ...

    - OPTIONAL - Open up `bootstrap.sh` in your text editor.
    - OPTIONALLY change `PASSWORD='root'` to something else
    - OPTIONALLY change the `PROJECTFOLDER='Project'` to the name of your wordpress installation or the name of the folder your project resides in.

3. In terminal cd into the project folder and use the following command to provision the box.
    ```
    cd /path/to/Project
    vagrant up
    ```
    Vagrant will download the necessary virtualbox image or vagrant box and initialize it with the commands listed in `bootstrap.sh`.

4. SSH into your vagrant by by typing the following command into your console (while in your project folder)
        vagrant ssh
Vagrant handles setting up the ssh credentials

5. All the contents of your project folder on your host computer will be available in /vagrant in the guest machine.  A symlink will also be created inside /var/www/html/project and an apache virtualhost will be added to point all web requests to the index within that folder.  If your project is structured differently, you will need to edit the apache virtual host file at /etc/apache2/sites-available.

6. Point the browser on your host computer to http://192.168.33.23 (or the ip address inside the Vagrant file.  Feel free to change it.)

7. After you're done with the machine, type vagrant suspend or vagrant halt into the terminal while under the project folder.  Suspend just suspends the virtual machine and saves the state, while halt shuts the machine off.  If you want to get rid of the machine, type vagrant destroy.  This will delete the vagrant box but your files within the project root will be safe.

### The files explained:
#### Vagrant File
Basic provisioning file for the vagrant box.  Points vagrant to the location of the virtual machine box to use.  In this case, ubuntu/trusty64 (14.04 LTS version).  It also configures any synced folders and the private network.

#### Bootstrap.sh
Shell script that is run by Vagrant on your new guest box.  If you need to install anything onto the machine during provisioning, add the shell command here.  Currently, this script installs the following:

    1. apache2
    2. php5.4
    3. mysql-server
    4. php5-mysql
    5. phpmyadmin
    6. git
    7. composer
    8. vim

Boostrap.sh also sets the root password for mysql and phpmyadmin to `root` (or change it in the file), sets up the apache virtualhost to point to the actual project folder within your root project folder (you can change the name in the file).


### Additional information:
- Getting Started https://www.vagrantup.com/docs/getting-started/
- Scripts based on this blog post https://www.dev-metal.com/super-simple-vagrant-lamp-stack-bootstrap-installable-one-command/
- Why should I care about vagrant https://24ways.org/2014/what-is-vagrant-and-why-should-i-care/
