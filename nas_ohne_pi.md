# NAS ohne PI

Einige haben ja [DIY NAS](https://www.fachinformatiker.de/blogs/entry/44-diy-nas/) gelesen und ich wurde auch schon gefragt ob da noch was passiert. Ja ein paar Dinge sind passiert. Diverse Gehäuseteile wurden gekauft einige wurden gedruckt. CPU Kühler für den Shuttle-Barbone wurden gekauft und modifiziert um auf dem Nano PI Platz zu finden. Und irgendwann ist man an dem Punkt das ganze mal testhalber einzuschalten und enttäuscht zu werden. Android läuft auf dem PI prima, die getesteten Linuxvarianten eher "meh". Der NanoPI versorgt nun mit Android 11, einem Touch-Display und ein paar Verstärkermodulen eine Küche mit Spotify, Audible, Youtube, ..
Aufmerksamen Lesern ist sicher nicht entgangen das es nun offenbar kein NAS gibt. 
Also ja schon, aber es ist nicht mehr so Low-Power wie anfangs gewollt war.

## Neue (alte) Hardware
Während and dem Barebone Gehäuse für den PI gebastelt wurde ist ein gebrauchter Supermicro Server eingezogen. Darauf liefen - oder auch nicht - zunächst einige Hypervisor (ESXi, HyperV, Proxmox). Die ursprüngliche Ausstattung war dann auch bald zu wenig und es gab einige Upgrades:
* 2x L5640 (6 Kern "Low-Power")
* 6x 16GB DDR3-1066 ECC REG

Anfang des Jahres hat sich dann das alte "Datengrab" - eine externe 4TB Platte - verabschiedet. Dies rückte das NAS Projekt mal wieder in den Fokus. Für einen niedrigen 4-stelligen Betrag wurden dann einige Festplatten besorgt.
Geplant war dann eigentlich den RAID-Controller an eine VM im Proxmox durchzureichen und dort Truenas zu installieren. Truenas beziehungsweise ZFS allgemein benötigt direkten Zugriff auf die Datenträger ohne einen RAID-Controller. HBA sind hier das Mittel zum Zweck, einige RAID-Controller kann in den IT-Mode umflashen. Leider mag der Controller weder den IT-Mode noch war das Passthrough wirklich erfolgreich.
Allerdings bietet Truenas Scale auch eine auf KVM basierte Virtualisierung und k8s basierte Container-Apps. Also weg mit Proxmox und Truenas direkt installiert, Glücklicherweise bot das Supermicro-Board genug SATA Ports um auf den Controller verzichten zu können. Beachten muss man hierbei, daß der Datenträger auf dem Truenas installiert wird nicht weiter zur Verfügung steht. Eine Installation auf einem USB-Stick ist nicht empfehlenswert, man benötigt also am besten eine kleine SSD zur Installation.

Nach der Installation und Ersteinrichtung des ZPools wird man von einem fröhlichen Dashboard begrüßt.
![nach der Erstinstallation und Einrichtung des ZPools](https://raw.githubusercontent.com/speisekatze/Blog/main/images/truenas_supermicro.png)

Für den Heimgebrauch kann man durchaus auf L2-ARC und slog-Device verzichten. Die Anforderungen für einen brauchbaren slog übersteigen die Fähigkeiten normaler SSDs ohnehin. Wenn man also nicht grad ein paar Enterprise-SSDs rumliegen hat, lässt man das besser. Der L2-ARC hilft nur wenn der ARC voll läuft oder zu klein für das Working-Set ist. Einen L2-ARC kann man einfach nachrüsten falls er wieder erwarten doch notwendig wird. Wobei selbst dann mehr RAM für einen größeren ARC sinnvoller ist.

## Was nun?
Einerseits hat das NAS nun mehr Leistung als mit dem NanoPi möglich gewesen wäre, dafür steht kein Proxmox Server mehr zur Verfügung. Wie schon gesagt bietet Truenas Scale eine rudimentäre Schnittstelle zu KVM Virtualisierung und App-Container über k8s, in Freenas und Truenas Core wird/wurde das über die FreeBSD eigenen Jails gelöst.
Der Truenas eigene App-Pool wächst immer weiter und bietet neben üblichen wie NextCloud und PLEX auch beispielsweise miniIO. Man kann aber auch eigene Container importieren oder den sehr umfangreichen [Truecharts Katalog](https://truecharts.org/docs/charts/description_list/) nutzen.

Für das Heimnetz stellt Truenas nun einige Dienste bereit
![Truenas APPs](https://raw.githubusercontent.com/speisekatze/Blog/main/images/truenas_services.png)

Über einen externen Proxy und VPN werden einige der Dienste auch nach außen freigegeben, so kann man beispielsweise den codeserver von jedem beliebigen Browser nutzen.
