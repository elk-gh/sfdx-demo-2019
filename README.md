### Demo CI DX - Dev Week

## Prerequisitos

- [Developer Org](https://developer.salesforce.com/signup)
- [Migration Toolkit](https://developer.salesforce.com/docs/atlas.en-us.daas.meta/daas/forcemigrationtool_install.htm)
- [Java y ANT](https://developer.salesforce.com/docs/atlas.en-us.daas.meta/daas/forcemigrationtool_prereq.htm)
- [Jenkins](https://jenkins.io/download/)
- [Visual Studio Code](https://code.visualstudio.com/)
- [Salesforce Extension Pack](https://marketplace.visualstudio.com/items?itemName=salesforce.salesforcedx-vscode)
- [Salesforce CLI](https://developer.salesforce.com/tools/sfdxcli)
- [OpenSSL](https://sourceforge.net/projects/gnuwin32/files/openssl/0.9.8h-1/openssl-0.9.8h-1-bin.zip/download)

## Demo 1 - Crear Objeto y Campo

- Ir a Objetos 
  - Crear Objetos
    Nombre: Cursos
- Ir a Campos 
  - Crear Campos
  Nombre: Créditos

## Demo 2

- Conectar Sandbox A y B
- Enviar Conjunto de cambios desde Sandbox A a B
- Implementar Cambios en Sandbox B

## Demo 3

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

## Demo 4

- Crear repositorio en Github
- Inicializar local
  ```
  git init
  git remote add origin <git-repository>
  git status
  git checkout -b <my-branch>
  git commit -m "commit message"
  git push -u origin <my-branch>
  ```
  
## Demo 5

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

## Demo 6

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
- Retrieve metadata (beta)
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
- Convertir source a mdapi
  ```
  sfdx force:source:convert --rootdir force-app --outputdir tmp_convert
  ```
- Zippear
- Autorizar Org B
  - SFDX: Authorize an Org
  ```
  sfdx force:auth:web:login --setalias myDevProd --instanceurl https://login.salesforce.com
  ```
- Deploy metadata
  - Sin Pruebas
  ```
  sfdx force:mdapi:deploy --zipfile tmp_convert.zip --targetusername myDevProd --testlevel NoTestRun -w 1
  ```
  - Con pruebas específicas
  ```
  sfdx force:mdapi:deploy --zipfile tmp_convert.zip --targetusername myDevProd --testlevel RunSpecifiedTests --runtests RTest -w 1
  ```

## Demo 7

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
  git clone git@github.com:elk-gh/sfdx-demo-2019.git
  git checkout demo-7
  ```
- Crear una Scratch Org
  ```
  sfdx force:org:create -s -f config/project-scratch-def.json -a sfdx-demo-2019
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

## Demo 8

- Crear un paquete no gestionado en A
- Agregar componentes
- Subir el paquete
- Compartir el link con B

## Demo 9

- Habilitar paquetes desbloqueados en A
- Clonar Repositorio
  ```
  git clone git@github.com:elk-gh/sfdx-demo-2019.git
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

## Demo 10
- Descargar openSSL en C:\openssl de https://sourceforge.net/projects/gnuwin32/files/openssl/
- Descomprimir y cambiar el nombre de la carpeta a openssl
- Copiar y pegar la carpeta openssl a C:\
- Setear variable de entorno (System Variable)
  ```
  OPENSSL_CONF=C:\openssl\share\openssl.cnf
  ```
- Ir a C:\openssl\bin y ejecutar CMD desde ahí
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
  ``` 
      Name: Jenkins
      Callback URL: http://localhost:1717/OauthRedirect
      Selected OAuth Scopes:
      - Access your basic information (id, profile, email, address, phone)
      - Access and manage your data (api)
      - Provide access to your data via the Web (web)
      - Perform requests on your behalf at any time (refresh_token, offline_access)
      Check Use digital signatures
      Choose File: server.crt en C:\openssl\bin
      Click en Manage
      Click en Edit Policies
      Permitted Users: Admin approved user are pre-authorized
      Click Save
      Click Manage Profiles
      Select System Administrator
      Click Save
  ```
- Validar JWT Login Flow desde C:\openssl\bin
  ```
    sfdx force:auth:jwt:grant --clientid {ADD_YOUR_CLIENT_ID} --jwtkeyfile server.key --username {ADD_YOUR_USERNAME} --instanceurl https://login.salesforce.com --setdefaultdevhubusername
  ```
  ```
    - clientid  :- provide Consumer Key
    - jwtkeyfile :- Absolute path to the location where you generated your OpenSSL server.key file
    - instanceurl :-provide instanceurl if you are using sandbox.
    - setdefaultdevhubusername :- Set Default dev hub User Name.
  ```
- Ir a Jenkins
- Configurar Server.key en el pluggin
  ```
    - Jenkins
    - Credentials
    - System
    - Global credentials (unrestricted) 
    - Add Credentials
    - Kind: Secret File
    - Seleccionar server.key en C:\openssl\bin
  ```
- Establecer las variables de Entorno del Sistema para Jenkins en el PC 
  ```
    HUB_ORG_DH: Username for the Dev Hub Org.
    SFDC_HOST_DH: Login URL of the Salesforce instance which is hosting the Hub. Example: https://login.salesforce.com
    CONNECTED_APP_CONSUMER_KEY_DH : Consumer key obtained in the connected app
    JWT_CRED_ID_DH: The credentials ID for the private  key file (Se obtiene desde Jenkins)
        - Login en Jenkins
        - Ubicar las credenciales globales (Credentials --> System --> Global Credentials --> ServerKey)
        - Update (Extraer ID del file y utilizarlo en la variable de entorno JWT_CRED_ID_DH)
   ```
- Configurar Jenkins
  ```
    - Jenkins --> New Item
    - Definir un nombre
    - Seleccionar Categoria Multibrach PipeLine
    - Branch sources --> Seleccionar Adicionar un item
    - Single repository & branch --> Indicar la URL del repositorio
    - Especificar la rama si es requerido
    - Guardar 
  ```  
- Ir a la pestaña del Item creado
- En la lista que aparece dar click en el nombre del ultimo build
- Selecionar la opcion "Build Now" en la iquierda
  ```
    Console outPut (Se evidencia que no se puede obtener la variable de entorno)
  ```  
- Reinciar el equipo
- Retornar a Jenkins
- Ir al repositorio 
- Selecionar la opcion "Build Now" --> Console OutPut

http://amitsalesforce.blogspot.com/2019/01/continuous-integration-using-jenkins-with-salesforceDx.html








