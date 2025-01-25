# Configuración de Entorno Tomcat con Vagrant

## Pasos de Instalación

### 1. Crear Máquina Virtual
```bash
vagrant init
```

### 2. Configuración del Vagrantfile
- Sistema Operativo: Debian Bullseye
- Puerto: 8080 (forwarded)

### 3. Instalamos los siguientes paquetes
- OpenJDK 11
- Tomcat 9
- Maven
- Git

### 4. Usuarios Tomcat que necesitamos crear
- `alumno` (admin)
  - Contraseña: 1234
- `deploy` (manager-script)
  - Contraseña: 1234

### 5. Proyecto de Ejemplo
- Despliegue automático con Maven

### 6. Clonamos un repositorio de ejemplo
- Repositorio: Rock Paper Scissors

### 7. Iniciamos Tomcat
```bash
sudo systemctl enable tomcat9
sudo systemctl restart tomcat9
```


## Acceso
- Tomcat Manager: `http://localhost:8080/manager/html`, `http://localhost:8080/host-manager/html`
- Credenciales: usuario `alumno`, contraseña `1234`