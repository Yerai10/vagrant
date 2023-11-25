# -*- mode: ruby -*-
# vi: set ft=ruby :

#Este codigo crea una maquina virtual automaticamente con 2 GB de RAM y 2CPUs, donde se instala Java 8 y Hadoop automaticamente 
#cuando se crea por primera vez, una vez que ya se haya levantado y la volvamos a levantar de nuevo no se ejecutará la provision
# y la maquina correrá correctamente. También he configurado el provision para conectarnos a la mv con Vagrant SSH desde consola
# por el Visual Studio Code a través del remote por SSH. 

Vagrant.configure("2") do |config| #Inicia la configuración de Vagrant, utilizando la versión 2 de la configuración.

  config.vm.box = "boxen/ubuntu-22.04-x86_64"  # Indicamos el box que queremos crear
  config.vm.define "mv_ejercicio" # El nombre que le estamos dando a la máquina virtual
  config.vm.boot_timeout = 600
  config.vm.provider "virtualbox" do |vb|  # Configuramos el hardware de la máquina    
    vb.memory = "2048" # Queremos 2Gb de RAM    
    vb.cpus = 2 # Seleccionamos 2 cpus

 ### Configuracion ssh:
    # Eliminar la regla NAT existente si existe
    vb.customize ["modifyvm", :id, "--natpf1", "delete", "ssh"]
    # Agregar la nueva regla NAT
    vb.customize ["modifyvm", :id, "--natpf1", "ssh,tcp,,2222,,22"]
    
  end
  ### Configuración red 
  config.vm.network "private_network", ip: "192.168.33.19" # Red privada con una dirección estática para poder acceder. 
 
  ### Configuracion ssh (Usuario y clave como False)
  config.ssh.username = "vagrant"  # El nombre de usuario
  config.ssh.insert_key = false
 
  ### Configuración provision solo correrá 1 vez instalando Java 8 y Hadoop
  config.vm.provision "shell", run: "once", inline: <<-SHELL
  sudo apt-get update
  sudo apt-get install -y openjdk-8-jre
  sudo systemctl enable openjdk-8-jre
  sudo systemctl start openjdk-8-jre
  
  wget https://downloads.apache.org/hadoop/common/hadoop-3.3.6/hadoop-3.3.6.tar.gz
  tar -xzvf hadoop-3.3.6.tar.gz
  sudo mv hadoop-3.3.6 /opt/hadoop
  sudo apt-get install -y hadoop 
  SHELL
end