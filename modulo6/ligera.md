
Clonación completa



virsh -c qemu:///system vol-clone template-debian.qcow2 prueba5.qcow2 --pool default


virt-install --connect qemu:///system \
			 --virt-type kvm \
			 --name prueba5 \
			 --os-variant debian10 \
			 --disk vol=default/prueba5.qcow2 \
			 --memory 1024 \
			 --vcpus 1

Y con virt-manager

Conación ligera

Crear volukmen con backing store

virsh -c qemu:///system vol-create-as default clone2.qcow2 10G --format qcow2 --backing-vol prueba1.qcow2 --backing-vol-format qcow2 


sudo qemu-img create -f qcow2 prueba6.qcow2 10G -b template-debian.qcow2

Con virt-manager

Y volvemos a crear una mv


