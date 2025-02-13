# Jasmin setup required for running an EPOC workflow

## Submission to Jasmin sci machines

In order to submit jobs to Jasmin from puma2 (for jdma and monitor tasks), 
you need to configure your ssh settings to connect to the Jasmin sci servers.   

1. Add the following to your `.ssh/config` file on PUMA2.
~~~
Host login-0?.jasmin.ac.uk
    User <JAMSIN-USERNAME>
    IdentityFile ~/.ssh/<JASMIN-SSH-KEY>
    ForwardAgent yes

Host sci-vm-0? sci-ph-0?
    User <JASMIN-USERNAME>
    IdentityFile ~/.ssh/<JASMIN-SSH-KEY>
    ForwardAgent yes
    ProxyJump login-03.jasmin.ac.uk
~~~
   * Replace the string `<JASMIN-USERNAME>` with your Jasmin username, e.g. `aosprey`.
   * Replace <JASMIN-SSH-KEY> with the name of your Jasmin key, e.g. `id_rsa_jasmin`. 
2. Add your Jasmin ssh-key to your ssh-agent:
   * Run `ssh-add ~/.ssh/<JASMIN-SSH-KEY>`
   * You will be prompted for your Jasmin ssh key passphrase. 
3. Test connection to Jasmin:
   * Try for example `ssh sci-vm-02`
   * You should be logged into the Jasmin sci node without prompt for your passphrase.
4. Add path to Rose/Cylc to your ~/.bash_profile on JASMIN:
~~~
if [[ $(hostname) = sci*.jasmin.ac.uk || $(hostname) = cylc*.jasmin.ac.uk || $(hostname) = host*.jc.rl.ac.uk ]]; then
  # Rose/cylc on jasmin-sci & Lotus nodes
  export PATH=/apps/jasmin/metomi/bin:$PATH
fi
~~~

## Data staging on Jasmin transfer cache 

The [Jasmin transfer cache](https://help.jasmin.ac.uk/docs/short-term-project-storage/xfc/) (XFC) is used in the EPOC workflows to stage data before migration to elastic tape, 
avoiding group workspace.

To setup an XFC space: 
1. First [install the XFC client](https://help.jasmin.ac.uk/docs/short-term-project-storage/install-xfc-client/)
2. Then follow the [initialisation steps](https://help.jasmin.ac.uk/docs/short-term-project-storage/xfc/#initial-setup)

## Migration of UM data to Jasmin elastic tape 

To setup your environment for migration of data to Jasmin Elastic Tape as part of the UM workflow:
1. Login to one of the Jasmin [sci servers](https://help.jasmin.ac.uk/docs/interactive-computing/sci-servers/)
2. Load the jdma client: 
~~~
. ~umshared/venvs/jdma_venv_py3/bin/activate
~~~
3. Follow the [setup steps](https://cedadev.github.io/jdma_client/docs/build/html/jdma_client/tutorial.html#setting-up-the-user-user-settings-and-user-info) in the JDMA documentation
