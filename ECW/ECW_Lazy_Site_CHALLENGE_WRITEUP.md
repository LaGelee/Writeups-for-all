# Lazy_Site
### European Cyber Week CTF

Le challenge n'est plus accessible donc je vais écrire avec ce dont je me souviens.

### Stats 
Completions 1/2  :  **73**

Completions 2/2  :  **37** 

Category         :  **Pentest**

### Lazy Site (1/2)
#### Description : This site is pretty much secure, because it has bearly no feature :D Or maybe? Server is available at https://allyourimages.challenge-ecw.fr.

En arrivant sur le site on a une page blanche avec une image d'un "donkey". En regardant le code source de la page on voit cette ligne "https://allyourimages.challenge-ecw.fr/ImageLoader/justlookatthis.aspx?img=donkey.png". Première chose intéressante c'est le format .aspx qui içi fait référence à un server Windows. Deuxièmement avec l'habitude ça fait directement penser à une LFI. Je peux donc télécharger quelques fichiers comme "./ImageLoader/justlookatthis.aspx" ou encore "./web.config". En cherchant sur internet je tombe sur cette [wordlist](https://gist.github.com/korrosivesec/a339e376bae22fcfb7f858426094661e) qui me permet de récupérer le fichier "./../../../../../Windows/System32/inetsrv/config/applicationHost.config". Le fichier contient entre autres ceci 

```xml
        <sites>
            <site name="Default Web Site" id="1">
                <application path="/">
                    <virtualDirectory path="/" physicalPath="%SystemDrive%\inetpub\wwwroot" />
                </application>
                <bindings>
                    <binding protocol="http" bindingInformation="*:80:" />
                </bindings>
            </site>
            <site name="allyourimages" id="441643884">
                <application path="/">
                    <virtualDirectory path="/" physicalPath="c:\inetpub\allyourimages" />
                </application>
                <bindings>
                    <binding protocol="http" bindingInformation="*:80:allyourimages.challenge-ecw.fr" />
                </bindings>
            </site>
            <site name="superstorage" id="1822011850">
                <application path="/">
                    <virtualDirectory path="/" physicalPath="c:\inetpub\superstorage_safe_folder" />
                </application>
                <bindings>
                    <binding protocol="http" bindingInformation="*:80:ultrasecure-hostname-superstorage.challenge-ecw.fr" />
                </bindings>
            </site>
            <siteDefaults>
                <logFile logFormat="W3C" directory="%SystemDrive%\inetpub\logs\LogFiles" />
                <traceFailedRequestsLogging directory="%SystemDrive%\inetpub\logs\FailedReqLogFiles" />
            </siteDefaults>
            <applicationDefaults applicationPool="DefaultAppPool" />
            <virtualDirectoryDefaults allowSubDirConfig="true" />
        </sites>
```

On observe donc qu'il existe un deuxieme site "ultrasecure-hostname-superstorage.challenge-ecw.fr". Par réflexe je viens récupérer son fichier web.config qui se trouve dans "./../../../../../../../inetpub/superstorage_safe_folder/web.config"

```xml
<!-- only use for debug purpose
      <appSettings>
             <add key="username" value="IISStorageUser"
             <add key="password" value="0NanhgWmClR42JhoHR1J"
      </appSettings>
 -->

```
Ces identifiants me permettent d'accéder au site et de récupérer le flag dans le code source de la page principale.

#### `Answer : ECW{Th4tw454w31rdl00k1ngd0nk3ey}`



### Lazy Site (2/2)
#### Description : No joke, this time it is really secure! I hope?

En regardant le fichier web.config de notre nouveau site on peut voir une référence à [WebDav](https://fr.wikipedia.org/wiki/WebDAV) qui permet de faire joujou avec les fichiers sur le server grâçe au protocole http. Le fichier général "applicationHost.config" nous donne plus d'informations.

```xml
    <location path="superstorage/App_Data">
        <system.webServer>
            <security>
                <requestFiltering>
                    <fileExtensions applyToWebDAV="false" />
                    <verbs applyToWebDAV="false" />
                </requestFiltering>
                <authentication>
                    <basicAuthentication enabled="true" />
                    <anonymousAuthentication enabled="false" />
                </authentication>
            </security>
            <webdav>
                <authoringRules>
                    <clear />
                    <add users="*" roles="" path="*.txt" access="Read, Write, Source" />
                </authoringRules>
            </webdav>
        </system.webServer>
    </location>
```
En gros, on peut créer des fichiers .txt sur le server en faisant des requêtes PUT en "http://ultrasecure-hostname-superstorage.challenge-ecw.fr/App_Data/mon_fichier.txt". Super ! Mais pas de Code Execution. Enfaite, ici rien ne précise que les .txt ne doivent être que des fichiers ! En faisans une requête avec l'option OPTIONS on voit que l'on peut utiliser l'option "MKCOL" qui permet de créer des dossiers. Donc je peux créer un dossier "mon_dossier.txt" et dans ce dossier je peux upload un reverse shell en .aspx.

Maintenant il suffit de parcourir le server pour trouver le flag. Le flag est lisible par l'utilisateur courant.

#### `Answer : NaN`

Write-up made wit :heart: by @LaGelee