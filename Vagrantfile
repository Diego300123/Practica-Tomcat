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

    # Creamos la configuracion de los usuarios de Tomcat
    sudo tee /etc/tomcat9/tomcat-users.xml > /dev/null <<EOF
<?xml version="1.0" encoding="UTF-8"?>
<tomcat-users xmlns="http://tomcat.apache.org/xml"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://tomcat.apache.org/xml tomcat-users.xsd"
version="1.0">
  <role rolename="admin"/>
  <role rolename="admin-gui"/>
  <role rolename="manager"/>
  <role rolename="manager-gui"/>
  <role rolename="manager-status"/>
  <role rolename="manager-script"/>
  <role rolename="manager-jmx"/>
   
  <user username="alumno"
    password="1234"
    roles="admin, admin-gui, manager, manager-gui"/>
</tomcat-users>
EOF

# Instalamos el Tomcat admin
sudo apt install -y tomcat9-admin

# Lo iniciamos y lo reiniciamos
sudo systemctl enable tomcat9
sudo systemctl restart tomcat9

      SHELL
end