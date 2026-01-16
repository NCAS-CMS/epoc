# Setup required to run an EPOC ensemble member

In order to run an EPOC workflow, you must have the following accounts:
* [Archer2 and Puma2](https://cms.ncas.ac.uk/archer2/unified-model/#getting-started)
* [MOSRS](https://code.metoffice.gov.uk/)
* [Jasmin](https://jasmin.ac.uk/users/access/)

You will then need to setup the following:  
* [Archer2 and puma2 environments to run the UM](https://ncas-cms.github.io/um-training/getting-setup-selfstudy.html)
* [Globus transfer from Archer2 to Jasmin](https://cms.ncas.ac.uk/unified-model/pptransfer-globus/) 
* alpaca GWS access on Jasmin:
  * Request alpaca GWS access via the [Jasmin accounts portal](https://accounts.jasmin.ac.uk/).
  * Then contact Jon to be made a GWS deputy
  * Then contact the Jasmin helpdesk for tape access.
* Cylc submission to Jasmin (see below)
* Jasmin transfer cache (see below)
* Data migration to Jasmin tape archive (see below)

## Cylc submission to Jasmin

In order to submit jobs to Jasmin from puma2 (for `jdma` and `monitor` tasks), 
you need to configure your ssh settings to connect to the Jasmin sci servers.   

1\. Add the following to your `.ssh/config` file on PUMA2.
{% raw %}
~~~
Host login.jasmin.ac.uk
    User <JAMSIN-USERNAME>
    IdentityFile ~/.ssh/<JASMIN-SSH-KEY>
    ForwardAgent yes

Host sci-vm-0? sci-ph-0?
    User <JASMIN-USERNAME>
    IdentityFile ~/.ssh/<JASMIN-SSH-KEY>
    ForwardAgent yes
    ProxyJump login.jasmin.ac.uk
~~~
{% endraw %}
   * Replace `<JASMIN-USERNAME>` with your Jasmin username, e.g. `aosprey`.
   * Replace `<JASMIN-SSH-KEY>` with the name of your Jasmin key, e.g. `id_rsa_jasmin`.
     
2\. Add your Jasmin ssh-key to your ssh-agent:
   * Run `ssh-add ~/.ssh/<JASMIN-SSH-KEY>`
   * You will be prompted for your Jasmin ssh key passphrase.
     
3\. Test connection to Jasmin:
   * Try for example `ssh sci-vm-02`
   * You should be logged into the Jasmin sci node without prompt for your passphrase.
     
4\. Add path to Rose/Cylc to your `~/.bash_profile` on JASMIN:
{% raw %}
~~~
if [[ $(hostname) = sci*.jasmin.ac.uk || $(hostname) = cylc*.jasmin.ac.uk || $(hostname) = host*.jc.rl.ac.uk ]]; then
  # Rose/cylc on jasmin-sci & Lotus nodes
  export PATH=/apps/jasmin/metomi/bin:$PATH
fi
~~~
{% endraw %}

## Jasmin transfer cache 

The [Jasmin transfer cache](https://help.jasmin.ac.uk/docs/short-term-project-storage/xfc/) (XFC) is used in the EPOC workflows to stage data before migration to elastic tape, 
avoiding group workspace.

To setup an XFC space: 
1. First [install the XFC client](https://help.jasmin.ac.uk/docs/short-term-project-storage/install-xfc-client/)
2. Then follow the [initialisation steps](https://help.jasmin.ac.uk/docs/short-term-project-storage/xfc/#initial-setup)

## Data migration to Jasmin tape archive

To setup your environment for migration of data to Jasmin Elastic Tape as part of the UM workflow:
1. Login to one of the Jasmin [sci servers](https://help.jasmin.ac.uk/docs/interactive-computing/sci-servers/)
2. Load the jdma client:
~~~
. ~umshared/venvs/jdma_venv_py3/bin/activate
~~~
3. Follow the [setup steps](https://cedadev.github.io/jdma_client/docs/build/html/jdma_client/tutorial.html#setting-up-the-user-user-settings-and-user-info) in the JDMA documentation

