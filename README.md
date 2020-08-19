# SELinux Policy for TURN Servers
This policy module provides support for TURN servers. Currently, only [coturn](https://github.com/coturn/coturn) support is available and tested.

This policy targets the Gentoo SELinux policy. Currently, systemd support is not available.

This policy should be considered in its infancy or in a testing stage and thus not recommended for production use. However, all typical functionality has been confirmed working.

## Installation
```
# Clone the repository
git clone https://github.com/0xC0ncord/turnserver-selinux.git

cd turnserver-selinux

# Compile the module for the desired policy type (i.e. strict)
make -f /usr/share/selinux/<policy_type>/include/Makefile

# Install the SELinux policy module
semodile -i turnserver.pp

# Set up ports
semanage port -a -t turnserver_port_t -p tcp 3478
semanage port -a -t turnserver_port_t -p tcp 3479
semanage port -a -t turnserver_port_t -p tcp 5349
semanage port -a -t turnserver_port_t -p tcp 5350
semanage port -a -t turnserver_port_t -p udp 3478
semanage port -a -t turnserver_port_t -p udp 3479
semanage port -a -t turnserver_port_t -p udp 5349
semanage port -a -t turnserver_port_t -p udp 5350

# Restore all correct context labels
restorecon -vF /usr/bin/turnserver
restorecon -vF /etc/turnserver.conf
restorecon -vF /var/log/turnserver.log
restorecon -RvF /run/turnserver

# Start the TURN server
/etc/init.d/turnserver start

# Ensure the TURN server is running in the correct confined context
ps -eZ | grep turnserver
```
