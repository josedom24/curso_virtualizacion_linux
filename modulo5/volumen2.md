



  525  cd /srv/images/
  526  ls
  527  ls -al
  528  qemu-img info vol1.qcow2 
  529  qemu-img create -f qcow2 vol2.qcow2 2G
  530  virsh vol-list prueba
  531  virsh pool-refresh prueba 
  532  virsh vol-list prueba
  533  virsh vol-info vol1.qcow2 prueba
