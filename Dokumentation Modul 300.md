# Dokumentation Modul 300

# Mateo Cukulin

###### Freitag, 05. Mai 2020

## Inhaltsverzeichnis

* Aufgabenstellung
* Docker File
* Docker Compose
* Unterschied zwischen dem Docker File und Docker Compose
* Fazit 

## Aufgabenstellung

Sie erstellen ein selbst gewähltesProjekt, welches auf der Docker Container-Technologie basiert.Dabei erstellen sie einen Service, derinnerhalb eines oder mehrerer Container implementiert wird.Anforderungen an den Service: •Prinzipiell muss der Service von ausserhalb des PC / VMgenutzt werden können, Bsp. mit einem Fat-Client(SQL-Client,Teamspeak usw.) oder einem Webbrowser.Ausnahmen müssen begründet sein.•Der Service soll entweder aus einem selber erstelltenDocker-Image oder aus einer Kombination von Docker-Files bestehen. In jedem Fall muss beschrieben sein, wie der Docker Service betrieben werden kann.•Sämtlicherverwendeter Code (auch Fremdcode) muss ausreichend dokumentiert sein.•Es muss dokumentiert sein, wie der Service funktioniert und benutzt werden kann.•Sie müssen mündlich über ihren Service Auskunft geben können.•Alle relevanten Files müssen auf GIT-Hub / GIT-Lab abgelegt und für die LP freigegebensein.

## Dockerfile

// FROM centos:latest

Mit dieser Zeile wird die neuste Version des Centos Docker Images benutzt. Man könnte auch anstelle von latest die Version angeben, die man gerne möchte. In meinem Fall ist die Neuste Verison Tip Top.

// MAINTAINER Mateo

Der Admin von diesem Dockerfile wird hier festgehalten. Deshalb steht nach MAINTAINER mein Name. Man kann hier auch den Firmannamen oder sonst was hinterlegen.

// RUN yum -y install httpd

RUN steht dazu da, dass ein folgender Command im Shell ausgeführt wird. Dieser obige Befehl installiert httpd. Unter httpd wird der WEbserver apache gemeint. Somit wird hier bei mir apache installiert.

// COPY index.html /var/www/html/

COPY ist ein Sehll Command welcher das File index.html in das gewünschte Verzeichnis Kopiert. Damit dies funktioniert, muss ein index.html file vorhanden sein. Ich habe hier den fehler gemacht, den Docker zu bilden, jedoch kommt anschliessend bei Punkt zwei eine Fehlermeldung. Nach langem suchen habe ich herausgefunden, dass ich zuerst mit sudo nano index.html eine solche Datei erstellen muss und einen gültigen html index einfügen muss. Anschliessend geht das installieren ohne Probleme weiter. 

// CMD ["/usr/sbin/httpd","-D","FOREGROUND"]

Mit dem CMD kann man einen Command ausführen, jedoch nur beim start des Containers.

// EXPOSE 80

Der Container wird durch EXPOSE auf den Port 80 hören. 


nachdem man dies im Dockerfile eingefügt hat, kann man mit sudo docker build -t apacheimage das Dockerfile erstellen. apacheimage ist der Name, das bedeutet das hier etwas frei gewählt werden kann. 

Um anschliessend im eigenen Lokalen Browser auf die WEbseite zu gelangen muss vorerst folgender Befehl durchgeführt werden. sudo docker run -dit -p 2345:80 apacheimage. Somit kann man anschliessend den Lokalen Browser öffnen und mit localhost:2345 sollte man ohne Probleme auf die WEbseite gelangen. 

![](//ss002207/TAACUMA2$/Desktop/image.png)

FROM centos:latest
MAINTAINER Mateo
RUN yum -y install httpd
COPY index.html /var/www/html/
CMD ["/usr/sbin/httpd","-D","FOREGROUND"]
EXPOSE 80


## Docker Compose 

// version: '2'
  
Hier wird die Version von Docker Compose angegeben. Ich benutze in diesem Fall die Version 2, welche nicht mehr die neuste ist. Auch hier könnte man 'latest' wählen, dadurch wird die neuste Version installiert.
  
// services:

Hier werden alle Dienste die untenstehend folgen, installiert und konfiguriert. Wie man unten im ganzen Code sehen kann, ist auch alles was nach services folgt eingeruckt und nicht auf der selben Ebene wie services.

// web:

Mit web wird angegeben, dass nun ein Webserver installiert werden soll. Unterhalb wird festgehalten welchen Webserver installiert wird.

// image: nginx

Hier wird das image festgelegt. In meinem Fall werde ich nginx als Webserver benutzen. man könnte jedoch auch hier httpd (apache) verwenden. Dies hängt davon ab, wass man bevorzut. Ich dachte jedoch dass ich nicht das gleiche in beiden Fällen habe werde ich hier nicht wieder apache verwenden. 

// ports: - 8080:80

Hier wird der Port 80 weitergeleitet auf den neuen Port 8080. Somit kann man anschliessend auf dem Lokalen Computer mit localhost:8080 die WEbseite aufrufen.

// volumes: - "./SharedFolder:/usr/share/nginx/html"

In diesem Ordner wird das Index File abgelegt. Das Index File beinhaltet die WEbseite. Jede Webseite verfügt über ein Index File. Zusätzlich kann auch eine CSS datei erstellt werden. Diese wird für das Design benötigt, kann aber auch gleich im HTML File angelegt werden. 

 version: '2'
  services:
    web:
      image: nginx
      ports: 
        - 8080:80
      volumes:
        - "./SharedFolder:/usr/share/nginx/html"
        
Hier sieht man wie oben bereits erwähnt, dass alles nach services eingeruckt ist. Somit ist klaar das alles zu den services gehört.

## Unterschid zweischen Docker File und Docker Compose

Ein Docekrfile wird verwendet um ein image zu erstellen. Dieses wird anschliessend verwendet um einen Container zu erstellen.

Bei einem Docker Compose wird gleich ein Container erstellt. DAs bedeutet dass man sein Dockerfile im Docker Compose einbinden könnte. Somit würde man mit Docker Compose einen Container erstellen, der mit ihrem selbergeschriebenen Dockerfile aufgesetzt wird. 

## index.html

<!DOCTYPE html>
<html lang="de">
<head>
        <meta charset="UTF-8">
        <title>Modul 300 LB3</title>
        <meta name="description" content="Kurzbeschreibung">
        <link href="design.css" rel="stylesheet">
</head>

<body>
Modul 300 / LB3 Testwebseite
</body>
</html>

Hier seht ihr noch mein default html index, den ich verwendet habe. Ich habe sie sehr striktgehalten und sie nur benutzt um alles ausführen zu können. Es ist auch eine Hilfe bei der Kontrolle ob der WEbserver erreichbar ist. Bei mir steht jetzt zum Beispiel Modul 300 / LB3 Testwebseite. Somit weiss ich gleich das es funktioniert hat. 


## Fazit

Es ist mir sehr schwer gefallen. Es gibt immer wieder Fehlermeldungen welche man anschliessend Googlen muss um weiter zu kommen. Damit verliert man sehr viel Zeit, die wir in diesem Modul nicht hatten, da 2 Tage ausgefallen sind. Ich bin selber mit mir sehr zufrieden, da ich anfangs gar keine Ahnung über das Thema hatte. Nun verstehe ich die grundliegenden Dinge und hoffe dies auch mal anwenden zu können. Ich konnte gerade jetzt in meiner Firma in eine Abteilung wechseln wo mit Puppet gearbeitet wird. Ich hoffe das ich nun einige Erfahrungen von hier direkt anwenden kann. 