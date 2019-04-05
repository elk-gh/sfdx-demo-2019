# sfdx-demo-2019
# Demo CI DX - Dev Week

## Prerequisitos

- [Developer Org](https://developer.salesforce.com/signup)
- [Migration Toolkit](https://developer.salesforce.com/docs/atlas.en-us.daas.meta/daas/forcemigrationtool_install.htm)
- [Java y ANT](https://developer.salesforce.com/docs/atlas.en-us.daas.meta/daas/forcemigrationtool_prereq.htm)
- [Jenkins](https://jenkins.io/download/)
- [Visual Studio Code](https://code.visualstudio.com/)
- [Salesforce Extension Pack](Salesforce Extension Pack)
- [Salesforce CLI](https://developer.salesforce.com/tools/sfdxcli)

## Demo 1 - Crear Objeto y Campo

- Ir a Objetos - Crear Objetos
Nombre: Cursos
- Ir a Campos - Crear Campos
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
   `ant retrieveUnpackaged
- Deploy a B
  `ant deployUnpackaged

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

SFDX: Authorize an Org
  ```
  sfdx force:auth:web:login --setalias myDev1 --instanceurl https://login.salesforce.com
  ```
set defaultusername
  ```
  sfdx force:config:set defaultusername=ecordoba-fp-01@avanxo.com.demo
  ```
- Retrieve metadata (beta)
  ```
  sfdx force:source:retrieve --metadata CustomObject:Curso__c,CustomField:Curso__c.Creditos__c,ApexClass
  ```
  ```
  sfdx force:source:retrieve -x path/to/package.xml (con manifest)
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
  SFDX: Authorize an Org
  ```
  sfdx force:auth:web:login --setalias myDevProd --instanceurl https://login.salesforce.com
  ```
- Deploy metadata
  ```
  sfdx force:mdapi:deploy --zipfile tmp_convert.zip --targetusername myDevProd --testlevel NoTestRun -w 1
  ```
  ```
  sfdx force:mdapi:deploy --zipfile tmp_convert.zip --targetusername myDevProd --testlevel RunSpecifiedTests --runtests RTest -w 1
  ```
## Demo 7

## Demo 8

## Demo 9

## Demo 10
