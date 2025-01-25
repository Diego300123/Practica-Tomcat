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
  <user username="deploy"
    password="1234"
    roles="manager-script"/>
</tomcat-users>
EOF

# Instalamos el Tomcat admin
sudo apt install -y tomcat9-admin



# Habilitamos el acceso remoto 
    sudo tee /usr/share/tomcat9-admin/host-manager/META-INF/context.xml > /dev/null <<EOF
<?xml version="1.0" encoding="UTF-8"?>
<Context antiResourceLocking="false" privileged="true" >
  <CookieProcessor className="org.apache.tomcat.util.http.Rfc6265CookieProcessor"
                   sameSiteCookies="strict" />
  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="\d+\.\d+\.\d+\.\d+" />
  <Manager sessionAttributeValueClassNameFilter="java\.lang\.(?:Boolean|Integer|Long|Number|String)|org\.apache\.catalina\.filters\.CsrfPreventionFilter\$LruCache(?:\$1)?|java\.util\.(?:Linked)?HashMap"/>
</Context>
EOF




# Instalamos Maven
sudo apt-get update && sudo apt-get -y install maven


# Configuramos Maven 
sudo sed -i '/<servers>/a\
<server>\
  <id>Tomcat</id>\
  <username>deploy</username>\
  <password>1234</password>\
</server>' /etc/maven/settings.xml


# Generamos una aplicación de prueba 
mkdir -p /home/vagrant/tomcat-project
cd /home/vagrant/tomcat-project
mvn archetype:generate -DgroupId=org.zaidinvergeles \
  -DartifactId=tomcat-war \
  -deployment \
  -DarchetypeArtifactId=maven-archetype-webapp \
  -DinteractiveMode=false

# Configuramos el fichero pom.xml
  cd sample-webapp
  sed -i '/<build>/a\
  <plugins>\
      <plugin>\
        <groupId>org.apache.tomcat.maven</groupId>\
        <artifactId>tomcat7-maven-plugin</artifactId>\
        <version>2.2</version>\
        <configuration>\
          <url>http://localhost:8080/manager/text</url>\
          <server>Tomcat</server>\
          <path>/rockpaperscissors</path>\
        </configuration>\
      </plugin>\
    </plugins>' pom.xml

  # Instalamos git
   sudo apt-get install -y git

  # Clonamos el repositorio del piedra-papel-tijeras
  git clone https://github.com/cameronmcnz/rock-paper-scissors.git /home/vagrant/rock-paper-scissors
  cd /home/vagrant/rock-paper-scissors
  git checkout patch-1

  # Cambiamos el usuario
  sudo chown -R vagrant:vagrant /home/vagrant/rock-paper-scissors
  
  # Lo iniciamos
  sudo systemctl enable tomcat9

  #  Reiniciamos tomcat
  sudo systemctl restart tomcat9

      SHELL
end