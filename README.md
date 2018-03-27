# Delegation Platform

Sets up what's needed for the CDS (Central Delegation Service).

Includes OpenAM, FakeMe, database (postgresql), test harness, demo app.

`vagrant up` gives you the whole environment, ready for CDS to be deployed into.

If you need to work behind a proxy, I suggest [vagrant-proxyconf plugin](https://github.com/tmatilai/vagrant-proxyconf)

## Ansible Vault
To deploy to poc-dev, you will need to run the ansible playbook and supply the ansible vault password. There is no value in encrypting the vagrant passwords so they have been left as is.
