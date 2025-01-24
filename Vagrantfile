Vagrant.configure("2") do |config|
  config.vm.box = "debian/bullseye64"
  
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = 2
  end
  
  config.vm.network "forwarded_port", guest: 8080, host: 8080

  config.vm.provision "shell", inline: <<-SHELL
  
    # Hacemos un update para comprobar que están todos los paquetes actualizados
    sudo apt-get update
    sudo apt-get upgrade -y
    
    # Instalamos OpenJDK
    sudo apt-get install -y openjdk-11-jdk

    # Instalamos Tomcat
    sudo apt install -y tomcat9

    #Creamos un grupo para el
    sudo groupadd tomcat9

    # Creamos el usuario
    sudo useradd -s /bin/false -g tomcat9 -d /etc/tomcat9 tomcat9

    # Iniciamos el servicio
    sudo systemctl start tomcat9


      SHELL
end