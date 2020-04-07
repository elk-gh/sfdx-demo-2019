## Demo 10
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
- Manage Profiles: System Administrator
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
  ```
    - Jenkins
    - Credentials
    - System
    - Add Credentials
    - Kind: Secret File
    - Seleccionar server.key
  ```
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
  ```
    - Jenkins --> New Item
    - Definir un nombre
    - Seleccionar Categoria Multibrach PipeLine
    - Branch sources --> Seleccionar Adicionar un item
    - Single repository & branch --> Indicar la URL del repositorio
    - Guardar 
  ```  
- Ir a la pestaña del Item creado
- Ir al repositorio 
- Selecionar la opcion "Build Now" 
  ```
    Console outPut (Se evidencia que no se puede obtener la variable de entorno)
  ```  
- Reinciar el equipo
- Retornar a Jenkins
- Ir al repositorio 
- Selecionar la opcion "Build Now" --> Console OutPut



