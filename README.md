# FOP4: Function Offloading Prototyping with P4

This fork of Mininet and Containernet allows to use Docker containers and P4-enabled devices like P4-switches and SmartNIC. This allows to build and experiment function offloading prototypes.

Based on: **Mininet 2.2.1**

* Containernet website: https://containernet.github.io/
* Mininet website:  http://mininet.org
* Original Mininet repository: https://github.com/mininet/mininet

---
## Cite this work

If you use FOP4 for your research and/or other publications, please cite (beside the original Mininet paper) the following paper to reference our work:

---
## Features

* Add, remove Docker containers to Mininet topologies
* Add, remove P4-enabled switches to Mininet topologies
* Add, remove eBPF-XDP hosts to Mininet topologies
* Connect Docker containers to topology (to switches, other containers, or legacy Mininet hosts)
* Execute commands inside Docker containers by using the Mininet CLI
* Dynamic topology changes (lets behave like a small cloud ;-))
   * Add Hosts/Docker containers/P4-switches/eBPF hosts to a *running* Mininet topology
   * Connect Hosts/Docker containers/P4-switches/eBPF hosts containers to a *running* Mininet topology
   * Remove Hosts/Docker containers/P4-switches/eBPF hosts containers/Links from a *running* Mininet topology
* Resource limitation of Docker containers
   * CPU limitation with Docker CPU share option
   * CPU limitation with Docker CFS period/quota options
   * Memory/swap limitation
   * Change CPU/mem limitations at runtime!
* Traffic control links (delay, bw, loss, jitter)
   * (missing: TCLink support for dynamically added containers/hosts)
* Automated unit tests for all new features
* Automated installation based on Ansible playbook

---
## Installation

FOP4 comes with three installation and deployment options.

### Option 1: Bare-metal installation

Automatic installation is provided through an Ansible playbook.
* Requires: **Ubuntu Linux 18.04 LTS** and **P4 tools installed** see P4_example folder
    ```bash
    $ sudo apt-get install ansible git aptitude
    $ git clone https://github.com/ANTLab-polimi/FOP4.git
    $ cd FOP4/ansible
    $ sudo ansible-playbook -i "localhost," -c local install.yml
    $ cd ..
    $ sudo python setup.py install
    ```
    Wait (and have a coffee) ...

### Option 2: Nested Docker deployment

Coming soon.

### Option 3: Vagrant-based VM creation

Coming soon.

### Usage / Run

Start example topology with some empty Docker containers connected to the network.

* `cd FOP4`
* run: `sudo python examples/containernet_example.py`
* use: `containernet> d1 ifconfig` to see config of container `d1`

### Topology example

In your custom topology script you can add Docker hosts as follows:

```python
info('*** Adding docker containers\n')
d1 = net.addDocker('d1', ip='10.0.0.251', dimage="ubuntu:trusty")
d2 = net.addDocker('d2', ip='10.0.0.252', dimage="ubuntu:trusty", cpu_period=50000, cpu_quota=25000)
d3 = net.addHost('d3', ip='11.0.0.253', cls=Docker, dimage="ubuntu:trusty", cpu_shares=20)
d4 = net.addDocker('d4', dimage="ubuntu:trusty", volumes=["/:/mnt/vol1:rw"])
```

You can add P4-enabled switch as follows:
```python
info('*** Adding P4 switches\n')
# add P4 switch with Thrift-based interface
s1 = net.addP4Switch(name="S1", netcfg=False,
    json="example_p4.json",
    switch_config="switch_config.cfg")

# add P4 switch controlled by ONOS
s2 = net.addP4Switch(name="S2",
    pipeconf="org.onosproject.pipelines.basic")
```

You can add eBPF Host as follows:
```python
info('*** Adding eBPF host\n')
enf1 = net.addEbpfHost("ENF1")
net.addLink(enf1, s2,
    ebpfProgram1="./ebpf_program.o")
```

### Tests

There is a set of Containernet specific unit tests located in `mininet/test/test_containernet.py`. To run these, do:

* `sudo py.test -v mininet/test/test_containernet.py`

---
## Contact

### Support

If you have any questions, please use GitHub's [issue system](https://github.com/ANTLab-polimi/FOP4/issues)

### Contribute

Your contributions are very welcome! Please fork the GitHub repository and create a pull request.

### Lead developers

Daniele Moro
* Mail <daniele (dot) moro (at) polimi (dot) it>
* Github: [@daniele-moro](https://github.com/daniele-moro)

Manuel Peuster
* Mail: <manuel (dot) peuster (at) upb (dot) de>
* GitHub: [@mpeuster](https://github.com/mpeuster)
* Website: [Paderborn University](https://cs.uni-paderborn.de/cn/person/?tx_upbperson_personsite%5BpersonId%5D=13271&tx_upbperson_personsite%5Bcontroller%5D=Person&cHash=bafec92c0ada0bdfe8af6e2ed99efb4e)
