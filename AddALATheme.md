# `ala-web-theme` Grails plugin #

The `ala-web-theme` Grails plugin provides the following features to a Grails app:

  * a default Sitemesh layout with ALA header & footer (including CSS, JS and images resources)
  * a default index page for the webapp
  * CAS support for auth.ala.org.au integration (including libs and web.xml additions)


---


## Get started ##

  * Create a blank Grails app (substituting in your new project name):

```
grails createApp TestApp
```

  * cd to TestApp
  * Edit `grails-app/conf/BuildConfig.groovy`
  * Add to the repositories sections

```
    repositories { 
    ...
        mavenRepo "http://maven.ala.org.au/repository/"  
    ...
}
```

  * Add the plugin as runtime e.g. (check the latest version via [maven repo](http://maven.ala.org.au/repository/org/grails/plugins/ala-web-theme/))

```
    plugins {
        ...
        runtime ":ala-web-theme:0.2.2"  
    }
```

  * Add the following standard block to the top of your Config.groovy, replacing the appName and ENV\_NAME values to suit your app.

```

/******************************************************************************\
 *  CONFIG MANAGEMENT
\******************************************************************************/
def appName = 'userdetails'
def ENV_NAME = "${appName.toUpperCase()}_CONFIG"
default_config = "/data/${appName}/config/${appName}-config.properties"
if(!grails.config.locations || !(grails.config.locations instanceof List)) {
    grails.config.locations = []
}
if(System.getenv(ENV_NAME) && new File(System.getenv(ENV_NAME)).exists()) {
    println "[${appName}] Including configuration file specified in environment: " + System.getenv(ENV_NAME);
    grails.config.locations.add "file:" + System.getenv(ENV_NAME)
} else if(System.getProperty(ENV_NAME) && new File(System.getProperty(ENV_NAME)).exists()) {
    println "[${appName}] Including configuration file specified on command line: " + System.getProperty(ENV_NAME);
    grails.config.locations.add "file:" + System.getProperty(ENV_NAME)
} else if(new File(default_config).exists()) {
    println "[${appName}] Including default configuration file: " + default_config;
    grails.config.locations.add "file:" + default_config
} else {
    println "[${appName}] No external configuration file defined."
}

println "[${appName}] (*) grails.config.locations = ${grails.config.locations}"
```

  * (Optionally `*`) Create a properties file in /data/{appName}/config/${appName}-config.properties or put the equivilent vars into the `grails-app/conf/Config.groovy` file:

```

# CAS Config
casProperties=casServerLoginUrl,serverName,centralServer,casServerName,uriFilterPattern,uriExclusionFilter,authenticateOnlyIfLoggedInFilterPattern,casServerLoginUrlPrefix,gateway,casServerUrlPrefix,contextPath
serverName=http://localhost:8080
contextPath=/mobileauth
casServerName=https://auth.ala.org.au
uriExclusionFilterPattern=/images.*,/css.*,/js.*,/less.*
casServerLoginUrl=https://auth.ala.org.au/cas/login
gateway=false
casServerUrlPrefix=https://auth.ala.org.au/cas
security.cas.logoutUrl=https://auth.ala.org.au/cas/logout

```

  * Use the bootstrap layout

```
mv grails-app/views/layouts/main.gsp.new grails-app/views/layouts/main.gsp
```

  * Run the app via the built-in tomcat:

```
grails run-app
```

and as the Grails output suggests, browse to http://localhost:8080/testApp.

Check http://maven.ala.org.au/repository/org/grails/plugins/ala-web-theme/ for the latest version of this plugin.

`*` If no `config.security.cas` settings are detected, then the plugin will "bypass" CAS.