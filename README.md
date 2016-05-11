# Runner : Ansible Workshop

This Ansible workshop is intended to cover the basics of developing a simple Ansible playbook.
As we develop this playbook we will adhere to the principle of seperation-of-concerns.  Aligning to this principle will allow us to develop key portions of our playbook independently of each other as well as execute key portions independently.  As part of this exercise we will also touch on best practices and development techniques.

In this workshop we will deploy an nginx server locally to your workstation.  We will be using Vagrant and Virtualbox for our infrastructure.  We will be using a custom vagrant module to define the provisioning tasks.  And we will use standard installation procedures to install and launch nginx on our newly provisioned infrastructure.

## Principles and Practices
- Principle: Seperation of Concerns
- Best Practice: Design using Roles and Test Locally
- Techniques: Leverage Roles for well defined tasks/actions.

## Pre-requisites
The pre-requisite instructions below assume that you are running a *nix system or some variant.  
### If You Are on Windows

>If you are running Windows please create a VM and then proceed.  If you are unable to create a VM then please proceed but consider the steps in this section as a guide and not the exact steps you will execute to install the required software.


### Software
#### Git
> We will be working with Git to download this workshop.  It's probably a safe assumption that you have Git installed if you are reading this document, but just incase that isn't the case you can find instructions for installing Git [here](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

#### Virtualbox
>Our infrastructure in this workshop will be running on Virtualbox systems.  Please go [here](https://www.virtualbox.org/wiki/Downloads) to download and install [Virtualbox](https://www.virtualbox.org/wiki/Downloads) for your OS.

#### vboxmanage - useful commands
Display network interfaces recognized by virtualbox:

`vboxmanage list bridgedifs`
> This information will be necessary when we define our local vbox system.

Display the OS types recognized by virtualbox:
`vboxmanage list ostypes`
> This information will be necessary when we define our local vbox system.

#### Vagrant
>We will be using Vagrant as the tool to provision our Virtualbox infrastructure.  Please go [here](https://www.vagrantup.com/downloads.html) to download and install [Vagrant](https://www.vagrantup.com/downloads.html) for your OS.

#### Python and pip
>Most systems these days have python and pip installed.  So for the most part I am going to assume that your system has this.  If your system doesn't then please take a look at the link below.
- Windows or Source :
[Download Python or an installer](https://www.python.org/download/releases/2.7/)

#### virtualenv
>[[*best practice*]] It is a highly recommended best practice to utilize virtual environments when developing with python.  We will carry this best practice forward into this workshop to create a custom and local python environment.

##### *Install* 
`pip install virtualenv`

A great reference doc for virtualenv can be found [here](http://docs.python-guide.org/en/latest/dev/virtualenvs/)

##### *Configure*
At this point lets create our workspace for this workshop.  Let's do this now before install Ansible.

`mkdir ~/workspace`

`cd ~/workspace`

Let's create a virtual space for our *R*unner *A*nsbile *W*orkshop:

`virtualenv raw`

Once created we then need to activate the virtual environment:

`cd raw`

`. ./bin/activate`

Now that the virtual environment is active we can now install Ansible local to this workspace.  Install Ansible here will not impact the standard system installation nor will it impact other work environments you might have.

#### Ansible
>Assumption:  Your *raw* virtual environment is active.

`pip install ansible`

Let's set some environment variables to improve our Ansible experience:

`export ANSIBLE_NOCOWS=1`

`export ANSIBLE_HOST_KEY_CHECKING=False`


## Best Practices
[Ansible best practices](http://docs.ansible.com/ansible/playbooks_best_practices.html) can be found [here](http://docs.ansible.com/ansible/playbooks_best_practices.html).

BP - Ansible uses some conventions to allow you to develop complex playbooks.
 
BP - Structure all your playbook projects according to the conventional [project hierarchy]().

BP - Seperate your infrastructure provisioning plays from your software installation/configuration plays.

## A Simple Playbook
`https://github.com/CenturyLinkCloud/rnr-ansible-meetup-may.git`
### All in One
The all-in-one playbook is a single playbook which tightly couples the building of the infrastructure with the provisioning of software.

#### Files
- nginx.yml

#### Execute
- `export ANSIBLE_HOST_KEY_CHECKING=False`
- `ansible-playbook -i localhost, nginx.yml`


#### Validate
http://198.168.2.10

#### Pros/Cons
- Pros
	- hmmm?
- Cons
	- Infrastructure and Software are tightly coupled.
	- Unable to test portions of the playbook independently.
		  
### A Playbook Project With Structure
The larger project here is actually our 'well structured' playbook.  Please take a look at the project structure below.  Notice how the building of the infrastructure is seperate from the installation of the software.

The Project structure:

	.
	├── build_server.yml
	├── library
	│   └── vagrant.py
	├── nginx.yml
	├── roles
	│   ├── build_server
	│   │   ├── defaults
	│   │   │   └── main.yml
	│   │   └── tasks
	│   │       └── main.yml
	│   ├── local_server
	│   │   ├── defaults
	│   │   │   └── main.yml
	│   │   └── tasks
	│   │       └── main.yml
	│   └── nginx
	│       ├── defaults
	│       │   └── main.yml
	│       ├── handlers
	│       │   └── main.yml
	│       ├── tasks
	│       │   └── main.yml
	│       └── templates
	│           ├── nginx.conf.j2
	│           └── super.conf.j2
	└── site.yml


#### Pros/Cons
- Pros
	- Concerns separated
	- Structured for reuse
- Cons
	- hmmm? 

### Devops: Infrastructure as Code
A necessary step towards moving this effort to a Devops model is to check in our Playbooks into a source code management system such as github.

#### Pros/Cons
- Pros
	- Immutable infrastructure
	- Change management
- Cons
	- hmmm?	 	 

## Helpful Hints
###Hint: -v
`-v` is the verbose switch for `ansible` and `ansible-playbook`.  But it doesn't just stop with one *v*.  You can go as far out as 4.  Each additional *v* will provide an extra level of detail.  Want to know what's going on or wrong then throw some *v*'s at Ansible.

####Hint: debug module
The debug module allows you to print out statements from your executing playbook.  Smart use of the debug module can help you understand output from tasks as well as execution flow.

####Hint: `ANSBILE_KEEP_REMOTE_FILES=1`
Without going to deep into the workings of Ansible - Ansible will generate execution scripts to execute on remote systems in your inventory.  These files are generally temporary and are removed when the task completes.  However, it can sometimes be helpful to inspect these files when you are experiencing issues.


## Resources
- [Ansbile](http://docs.ansible.com/ansible/)
- [Ansible Best Practices](http://docs.ansible.com/ansible/playbooks_best_practices.html)
- [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- [Vagrant](https://www.vagrantup.com/downloads.html)
- [Virtualbox](https://www.virtualbox.org/wiki/Downloads)
- [Python Virtual Environments](http://docs.python-guide.org/en/latest/dev/virtualenvs/)
