# mc.computecanada.dev

This project uses [Ansible MC Hub](https://github.com/ComputeCanada/ansible-mc-hub) to deploy the [MC Hub](https://github.com/ComputeCanada/mc-hub) web application on [mc.computecanada.dev](https://mc.computecanada.dev). It adds the required encrypted configuration files (including API keys and cryptographic keys) to deploy the full application.

> If you are not an employee of Compute Canada or are not authorized to deploy to `mc.computecanada.dev`, you may instead setup your own web server with [Ansible MC Hub](https://github.com/ComputeCanada/ansible-mc-hub) or simply [MC Hub](https://github.com/ComputeCanada/mc-hub).

## Getting the required deployment authorizations

In order to deploy and maintain `mc.computecanada.dev`, you need to send a private message to Frédéric Fortier-Chouinard (me) on Slack or open an issue on Github.

First, in order to access the secret files, you will need to own a GPG key pair and send your public key in order to decrypt the secret files with git-crypt.

Then, you can ask to have your SSH key pair authorized to deploy the Ansible playbooks to the VM instance of `mc.computecanada.dev`. To do so, you will need to also provide your SSH public key.

## Controller requirements

* [Ansible >= 2.9.11](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
* [GPG 2](https://gnupg.org/index.html)
* [git-crypt >= 0.6.0](https://www.agwa.name/projects/git-crypt/)

## Project setup

1. Clone this repository.
    ````
    git clone --recurse-submodules https://github.com/ComputeCanada/mc.computecanada.dev
    ````

2. Decrypt the configuration files.

    ````
    git-crypt unlock
    ````
    > git-crypt uses your GPG private keys to try to decrypt the content of secret files. Make sure GPG has access to a private key that is authorized by the project. Otherwise, decryption will fail.

## Authorizing users with LDAP

This project only allows access to users that have the LDAP attribute `ccServiceAccess` set to `cc_mchub`.

## Running the playbook

1. Start an SSH agent and add the private key of the host CentOS instance.

    ````bash
    eval `ssh-agent`
    ssh-add <SSH_PRIVATE_KEY_FILE>
    ````

2. Optionally, make changes to the playbook, inventory or configuration files.

3. Run the Ansible playbook.

    ````bash
    ansible-playbook -i hosts.yml ansible-mc-hub/site.yml
    ````

3. Navigate to `mc.computecanada.dev` and make sure everything is working correctly.

> If you ever need to make changes to a configuration file, make the modification and run the Ansible playbook again. You may need to restart the instance too.
