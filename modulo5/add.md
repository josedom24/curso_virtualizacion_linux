# Añadir nuevos discos a una máquina virtual

virsh attach-disk bullseye1 /srv/images/vol2.qcow2 vdb --driver=qemu --type disk --subdriver qcow2 --persistent
