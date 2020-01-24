# OCP4 Install config helper

The idea of the OCP4 install config helper is simply to show how to automate the steps of customizing your install config. This is a POC tool and not a finished product. This helper also requires some gathering of info. You will need to have some things ready for the variables we use in the template.

- Assumptions<br />
   - You are working with AWS cloud provider<br />
   - You are familiar with OpenShift install config file format and items<br />
- Requirements<br />
   - base domain<br />
   - cluser name<br />
   - machineCIDR<br />
   - aws region<br />
   - OpenShift pull secret<br />
   - A valid ssh public key (one that has matching private key in `~/.ssh`)

## Running the playbook
- Using this playbook<br />
  - Create a directory for install files<br />
    **`mkdir -p install-files`**

  - Tell openshift to create the install-config<br />
    **`openshift-install create install-config --dir install-files`**

  - Get the playbooks folder clned into the install-config directory<br />
    **`cd install-files`**<br />
    **`git clone https://github.com/ddreggors/ocp4-install-config-helper.git`**

  - Backup your original install-config.yaml<br />
    **`mv -v {,original-}install-config.yaml`**

  - Edit the vars.yml, public_key.pub, and pull-secret.txt files (all under `install-files/ocp4-install-config-helper/vars`)<br />
    - You will need to set the appropriate values in the vars.yaml file.<br />
    - You will need to copy the contents of your public key (often `~/.ssh/id_rsa.pub`) into the public_key.pub file<br />
    - Finally you need to paste the contents of the downloaded pull secret into the pull-secret.txt file

  - To run the playbook as is and use the install-config.yaml<br />
    - Make sure you are in the `install-files` directory<br />
    **`sudo ansible-playbook ocp4-install-config-helper/install-helper.yaml`**<br />
    - Once this playbook finishes you will have a new `install-config.yaml` file that will have all of your variables set properly in your `install-files` directory.

  - Finally we can use the new install-config as usual<br />
    **`cd .. && openshift-install create cluster --dir=./install-files`**
