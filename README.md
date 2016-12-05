# Description

This provides a plugin to enable lombok in eclipse che.


## How to build plugin-lombok


### 1- Change Version

change version to your current eclipse che version in

	plugin-lombok/che-plugin-lombok-server/pom.xml
	plugin-lombok/pom.xml

files

### 2- Copy plugin folder to eclipse che plugins

put the repo inside plugins folder of che source

add '  <module>plugin-lombok</module>' in plugins/pom.xml


You can insert the module anywhere in the list. After you have inserted it, run `mvn sortpom:sort` and maven will order the pom.xml for you.

### 3- Link to WS-Agent assembly

Introduce the server part of the extension as a dependency in `/che/assembly/assembly-wsagent-war`. 

Add: 


<dependency>
     <groupId>org.eclipse.che.plugin</groupId>
   <artifactId>che-plugin-lombok-server</artifactId>
    <exclusions>
        <exclusion>
            <artifactId>lombok</artifactId>
            <groupId>org.projectlombok</groupId>
        </exclusion>
    </exclusions>
</dependency>


You can insert the dependency anywhere in the list. After you have inserted it, run `mvn sortpom:sort` and maven will order the pom.xml for you.

### 4- Add dependancy to che-parent


<dependency>
    <groupId>org.eclipse.che.plugin</groupId>
    <artifactId>che-plugin-lombok-server</artifactId>
    <version>${che.version}</version>
</dependency>


You can insert the dependency anywhere in the list. After you have inserted it, run `mvn sortpom:sort` and maven will order the pom.xml for you.

### 4- Rebuild Eclipse Che


```Shell
## Build a new IDE.war
### This IDE web app will be bundled into the assembly
cd che/assembly/assembly-ide-war
mvn clean install

## Create a new web-app that includes the server-side extension
cd che/assembly/assembly-wsagent-war
mvn clean install

## Creates a new workspace agent that includes new web app w/ your extension
cd assembly/assembly-wsagent-server
mvn clean install

## Create a new Che assembly that includes all new server- and client-side extensions
cd assembly/assembly-main
mvn clean install
```



### 4- Run Eclipse Che

```Shell
## Start Che using the CLI with your new assembly
### Replace <version> with the actual directory name
export CHE_ASSEMBLY=path_to_che_sources/assembly/assembly-main/target/eclipse-che-<version>/eclipse-che-<version>
che start
```


### Documentation resources

- IDE Setup: https://eclipse-che.readme.io/v5.0/docs/setup-che-workspace  
- Building Extensions: https://eclipse-che.readme.io/v5.0/docs/create-and-build-extensions
- Run local Eclipse Che binaries: https://eclipse-che.readme.io/v5.0/docs/usage-docker#local-eclipse-che-binaries
