### Demo CI DX - Dev Week 06 Abril 2019

## Prerequisitos

- [Developer Org](https://developer.salesforce.com/signup)
- [Migration Toolkit](https://developer.salesforce.com/docs/atlas.en-us.daas.meta/daas/forcemigrationtool_install.htm)
- [Java y ANT](https://developer.salesforce.com/docs/atlas.en-us.daas.meta/daas/forcemigrationtool_prereq.htm)
- [Jenkins](https://jenkins.io/download/)
- [Visual Studio Code](https://code.visualstudio.com/)
- [Salesforce Extension Pack](https://marketplace.visualstudio.com/items?itemName=salesforce.salesforcedx-vscode)
- [GIT](https://git-scm.com/downloads)
- [Salesforce CLI](https://developer.salesforce.com/tools/sfdxcli)
- [OpenSSL](https://sourceforge.net/projects/gnuwin32/files/openssl/0.9.8h-1/openssl-0.9.8h-1-bin.zip/download)

### Comandos para verificar instalación

- Git              
  ```git --version```
- Java             
  ```java -version```
- Ant              
  ```ant -version```
- SFDX CLI 
  - Version ```sfdx --version```
  - Update ```sfdx update```
- OpenSSL          
  ```openssl version```

## Slides
 - [Salesforce DX - CI Workshop DeWeek 2019](https://slidr.io/elk-gh/salesforce-dx-ci-workshop-deweek-2019#1)
 
## Demo 1 - Crear Metadata

- Ir a Objetos 
  - Crear Objetos
    Nombre: Cursos
- Ir a Campos 
  - Crear Campos
  Nombre: Créditos

## Demo 2 - Change Set Development

- Conectar Sandbox A y B
- Enviar Conjunto de cambios desde Sandbox A a B
- Implementar Cambios en Sandbox B

## Demo 3 - ANT Migration Toolkit

- Instalar Migration Toolkit
- Instalar Java y ANT
- Setear variables de entorno
- Crear package.xml
- Retrieve desde A
  ```
  ant retrieveUnpackaged
  ```
- Deploy a B
  ```
  ant deployUnpackaged
  ```
Ver: [Deploying Metadata with the Force.com Migration Tool](https://www.youtube.com/watch?v=YW9aPrxvK3A)

## Demo 4 - Integrar Control de Versiones

- Crear repositorio en Github
- Configurar Usuario 
  ```
  git config --global user.name "YOUR_NAME"
  git config --global user.email "YOUR_EMAIL"
  ```
- Inicializar local
  ```
  git init
  git remote add origin <git-repository>
  git status
  git checkout -b <my-branch>
  git commit -m "commit message"
  git push -u origin <my-branch>
  ```
  
## Demo 5 - ANT + CVS + Jenkins

- Instalar Jenkins
- Hacer cambio en repositorio local
- Hacer push
  ```
  git checkout <my-branch>
  git add *
  git commit -m "commit message"
  git push -u origin <my-branch>
  ```
- Se detecta el cambio y se lanza el build

## Demo 6 - SFDX + CVS sin Scratch Orgs

- Instalar VS Code
- Instalar Extension Pack
- Instalar Salesforce CLI
- Crear Proyecto
  - SFDX: Create Project
  ```
  sfdx force:project:create --projectname mywork
  ```
- Crear .gitignore
  ```
  .sfdx
  .vscode
  .DS_Store
  ```
- Inicializar repositorio
  ```
  git init
  git remote add origin <git-repository>
  git checkout -b <my-branch>
  ```
- Autorizar Org A
  - SFDX: Authorize an Org
  ```
  sfdx force:auth:web:login --setalias myDev1 --instanceurl https://login.salesforce.com
  ```
  - set defaultusername
  ```
  sfdx force:config:set defaultusername=<username>
  ```
- Retrieve metadata (beta) (trae metadata en "source" format)
  - Por componentes
  ```
  sfdx force:source:retrieve --metadata CustomObject:Curso__c,CustomField:Curso__c.Creditos__c,ApexClass
  ```
  - Con Manifest
  ```
  sfdx force:source:retrieve -x path/to/package.xml
  ```
- Hacer commit
  ```
  git add *
  git commit -m "commit message"
  git push -u origin <my-branch>
  ```
- Convertir source a mdapi (Convierte del formato "source" al típico "metadata")
  ```
  sfdx force:source:convert --rootdir force-app --outputdir tmp_convert
  ```
- Zippear
- Autorizar Org B
  - SFDX: Authorize an Org
  ```
  sfdx force:auth:web:login --setalias myDevProd --instanceurl https://login.salesforce.com
  ```
- Deploy metadata (Se necesita el formato "metadata" para usar comandos de mdapi. Es el recomendado para producción hasta el momento)
  - Sin Pruebas
  ```
  sfdx force:mdapi:deploy --zipfile tmp_convert.zip --targetusername myDevProd --testlevel NoTestRun -w 1
  ```
  - Con pruebas específicas
  ```
  sfdx force:mdapi:deploy --zipfile tmp_convert.zip --targetusername myDevProd --testlevel RunSpecifiedTests --runtests RTest -w 1
  ```
Ver: [Convert and Deploy an Existing App](https://trailhead.salesforce.com/en/content/learn/modules/sfdx_app_dev/sfdx_app_dev_deploy)

## Demo 7 - SFDX + Scratch Org

- Activar DevHub en A
- Autorizar DevHub
  ```
  sfdx force:auth:web:login -d -a DevHub
  ```
- Set Default Dev Hub Username
  ```
  sfdx force:config:set defaultdevhubusername=ecordoba@avanxo.com.demodwdevhub
  ```
- Clonar Repositorio
  ```
  git clone git@github.com:elk-gh/sfdx-demo-2019.git folder-name
  git checkout demo-7
  ```
- Crear una Scratch Org
  ```
  sfdx force:org:create -s -f config/project-scratch-def.json -a sfdx-demo-2019 -v TARGETDEVHUBUSERNAME
  ```
- Hacer push a Scratch Org
  ```
  sfdx force:source:push
  ```
- Abrir Scratch Org
  ```
  sfdx force:org:open
  ```
Ver: [Quick Start: Salesforce DX](https://trailhead.salesforce.com/content/learn/projects/quick-start-salesforce-dx?trail_id=sfdx_get_started)

## Demo 8 - Paquete No Gestionado

- Crear un paquete no gestionado en A
- Agregar componentes
- Subir el paquete
- Compartir el link con B

## Demo 9 - Paquete Desbloqueado y Versionamiento

- Habilitar paquetes desbloqueados en A
- Clonar Repositorio
  ```
  git clone git@github.com:elk-gh/sfdx-demo-2019.git folder-name
  git checkout demo-9
  ```
- Crear paquete desbloqueado
  ```
  sfdx force:package:create --name unlockedDemo --description "My Package" --packagetype Unlocked --path force-app --nonamespace --targetdevhubusername DevHub
  ```
- Editar sfdx-project.json
 - Cambiar la versionName a Version 1.0, y la versionNumber a 1.0.0.NEXT.
- Crear la versión
  ```
  sfdx force:package:version:create -p unlockedDemo -d force-app -k test1234 --wait 10 -v DevHub
  ```
- Crear una Scratch Org
  ```
  sfdx force:org:create --definitionfile config/project-scratch-def.json --durationdays 30 --setalias MyScratchOrg -v DevHub
  ```
- Instalar paquete desbloqueado
  ```
  sfdx force:package:install --wait 10 --publishwait 10 --package unlockedDemo@1.0.0-1 -k test1234 -r -u MyScratchOrg
  ```
- Abrir Scrtach Org
  ```
  sfdx force:org:open -u MyScratchOrg
  ```
Ver: [Unlocked Packages for Customers](https://trailhead.salesforce.com/content/learn/modules/unlocked-packages-for-customers/build-your-first-unlocked-package)

## Demo 10 - Jenkins + SFDX
- Instalar openSSL en C:\openssl
- Setear variable de entorno
  ```
  OPENSSL_CONF=C:\openssl\share\openssl.cnf
  ```
- Ir a C:\openssl\bin
- Generar llave privada RSA
  ```
  openssl genrsa -des3 -passout pass:x -out server.pass.key 2048
  ```
- Crear archivo key del archivo server.pass.key 
  ```
    openssl rsa -passin pass:x -in server.pass.key -out server.key
  ```
- Generar el Certificate Signing Request.
  ```
    openssl req -new -key server.key -out server.csr
  ```
- Generar el Certificado SSL
  ```
      openssl x509 -req -sha256 -days 365 -in server.csr -signkey server.key -out server.crt
  ```
- Crear Aplicación Conectada
  ```- Callback URL: http://localhost:1717/OauthRedirect
  ```
- Enable OAuth Settings
  ```
      - Access your basic information (id, profile, email, address, phone)
      - Access and manage your data (api)
      - Provide access to your data via the Web (web)
      - Perform requests on your behalf at any time (refresh_token, offline_access)
  ```
- Use digital signatures
  ```
    server.crt
  ```
-Click en Manage
-Edit Policies
  ```
    Permitted Users: Admin approved user are pre-authorized
  ```
- Manage Profiles
  ```
  System Administrator
  ```
- Validar JWT Login Flow desde C:\openssl\bin
  ```
  fdx force:auth:jwt:grant --clientid {ADD_YOUR_CLIENT_ID} --jwtkeyfile server.key --username amit.salesforce21@gmail.com --instanceurl https://login.salesforce.com --setdefaultdevhubusername
  ```
  ```
    - clientid  :- provide Consumer Key
    - jwtkeyfile :- Absolute path to the location where you generated your OpenSSL server.key file
    - instanceurl :-provide instanceurl if you are using sandbox.
    - setdefaultdevhubusername :- Set Default dev hub User Name.
  ```
- Ir a Jenkins
- Configurar Server.key en el pluggin
    - Jenkins
    - Credentials
    - System
    - Add Credentials
    - Kind: Secret File
    - Seleccionar server.key
- Establecer las variables de Entorno de Jenkins
  ```
    HUB_ORG_DH: Username for the Dev Hub Org.
    SFDC_HOST_DH: Login URL of the Salesforce instance which is hosting the Hub
    CONNECTED_APP_CONSUMER_KEY_DH : Consumer key obtained in the connected app
    JWT_CRED_ID_DH: The credentials ID for the private  key file (Se obtiene desde Jenkins)
        - Login en Jenkins
        - Ubicar las credenciales globales (Credentials --> System --> Global Credentials --> ServerKey)
        - Update (Extraer ID del file y utilizarlo en la variable de entorno JWT_CRED_ID_DH)
   ```
- Configurar Jenkins
    - Jenkins
    - New Item
    - Definir un nombre
    - Seleccionar Categoria Multibrach PipeLine
    - Branch sources --> Seleccionar Adicionar un item
    - Single repository & branch --> Indicar la URL del repositorio
    - Guardar 
- Ir a la pestaña del Item creado
- Ir al repositorio 
- Selecionar la opcion "Build Now" 
- Ir a Console Output
- Si hay error, reinciar el equipo
- Si hay error correr el servicio de Jenkins como el Usuario Loggeado
- Retornar a Jenkins
- Ir al repositorio 
- Selecionar la opcion "Build Now" --> Console OutPut

Ver: 
- [Develop an App with Salesforce CLI and Source Control](https://trailhead.salesforce.com/en/content/learn/projects/develop-app-with-salesforce-cli-and-source-control)
- [Continuous Integration|Salesforce DX Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_ci.htm)
 
## Enlaces Adicionales
  - [Dude, where's my permission?](http://www.salesforcehacker.com/2013/05/dude-wheres-my-permission.html)
  - [Metadata Coverage](https://developer.salesforce.com/docs/metadata-coverage)




