# CoScale Ansible example

This Ansible playbook provides an example of installing CoScale into RPM or APT based environments. 

## Configuration and installation

To run the playbook you first need to set some configuration variables. In group_vars you will find two config files, `apt-servers.yml` and `rpm-servers.yml` one for each of the possible installation methods. These contain the variables that need to be set for the CoScale installation to succeed. Choose the one that applies to your server environment. For Redhat or Centos, change the `rpm-servers.yml` file. For Debian or Ubuntu, change the `apt-servers.yml` file.

    coscale:
        app_id: replace_me
        accesstoken: replace_me
        agent_template: replace_me

Before running the Ansible playbook you need to replace the values of these variables. The `app_id` you can find in your CoScale application by going to 'Users' and then 'Accesstokens' in the sidebar. You can find the `app_id` or application ID at the top of the page. 

To find your agent_template id and accesstoken you first have to create a custom agent. This can be done by going to the 'Datasources' > 'Agent' in the sidebar of your application and clicking the 'Create new CoScale agent' button. 

* In the first step it's important to select the "Package / Executable" option, as you are going to install the CoScale agent as a package. 
* In the second step you need to select the Operating system of the server you want to install CoScale on. 
* In the third step you need to enable the "(ADVANCED) I will manage my plugins on my servers instead of through the UI." checkbox at the bottom of the plugin list. This will tell our system that you are planning on configuring the agent through config management.
* You can continue the process until the final step, where you will get the installation instructions.

You should get a curl command that looks something like this: `curl 'https://app.coscale.com/api/v1/app/01235fe1-7232-123s-97a7-21c3f12b3f7f/agenttemplates/0015/download/?token=132dbdd6-xxxx-4013-a17b-0fc2aa00c7a3' > coscale-RedHat-5-agent.rpm && sudo rpm -iUvh coscale-RedHat-5-agent.rpm`. 

In this command, after the `agenttemplates/` part of the url, you will notice a number (0015). This is a unique number for the agent that you have created and is also the number that you need to enter for the `agent_template` value.

In the same command, after the agent template, you should also notice `token=....`, this is the value of the `accesstoken` variable. Copy the value into the configuration.

The end result of your `apt-servers.yml` or `rpm-servers.yml` should like something like this.

    coscale:
        app_id: 01235fe1-7232-123s-97a7-21c3f12b3f7f
        accesstoken: 132dbdd6-xxxx-4013-a17b-0fc2aa00c7a3
        agent_template: 0015

The next step is to add your own hosts to the `hosts` file, edit the `site.yml` to fit your preference, and run the playbook `ansible-playbook -i hosts --diff site.yml`. After the ansible-playbook run finishes you should see your agents appear on the Agents page. 

If you have any questions, don't hesitate to contact our support through support@coscale.com.
