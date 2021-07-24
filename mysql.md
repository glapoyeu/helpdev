##  Solución Mysql Workbench en Ubuntu 20.04 An AppArmor policy prevents this sender

Al actualizar mi ubuntu a la versión 20.04, me resultó este error al querer conectarme a Mysql por medio de Workbench. La explicación de esta situación es que Workbench usa conexiones ssh y Password Manager para funcionar correctamente. Por lo tanto, es necesario otorgar este permiso explícita mente.  

Para proporcionar los permisos es necesario colocar en la terminal estos dos comandos:   
snap connect mysql-workbench-community:password-manager-service  
snap connect mysql-workbench-community:ssh-keys
