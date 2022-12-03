# NAS ohne PI

Einige haben ja [DIY NAS](https://www.fachinformatiker.de/blogs/entry/44-diy-nas/) gelesen und ich wurde auch schon gefragt ob da noch was passiert. Ja ein paar Dinge sind passiert. Diverse Gehäuseteile wurden gekauft einige wurden gedruckt. CPU Kühler für den Shuttle-Barbone wurden gekauft und modifiziert um auf dem Nano PI Platz zu finden. Und irgendwann ist man an dem Punkt das ganze mal testhalber einzuschalten und enttäuscht zu werden. Android läuft auf dem PI prima, die getesteten Linuxvarianten eher "meh". Der NanoPI versorgt nun mit Android 11, einem Touch-Display und ein paar Verstärkermodulen eine Küche mit Spotify, Audible, Youtube, ..
Aufmerksamen Lesern ist sicher nicht entgangen das es nun offenbar kein NAS gibt. Doch, aber es ist nicht mehr so Low-Power wie anfangs gewollt war.

## Neue (alte) Hardware
Während and dem Barebone Gehäuse für den PI gebastelt wurde ist ein gebrauchter Supermicro Server eingezogen. Darauf liefen - oder auch nicht - zunächst einige Hypervisor (ESXi, HyperV, Proxmox). Die ursprüngliche Ausstattung war dann auch bald zu wenig und es gab einige Upgrades:
* 2x L5640 (6 Kern "Low-Power")
* 6x 16GB DDR3-1066 ECC REG

Anfang des Jahres hat sich dann das alte "Datengrab" - eine externe 4TB Platte - verabschiedet. Dies rückte das NAS Projekt mal wieder in den Fokus. Für einen niedrigen 4-stelligen Betrag wurden dann einige Festplatten besorgt.
Geplant war dann eigentlich den RAID-Controller an eine VM im Proxmox durchzureichen und dort Truenas zu installieren. Leider mag der Controller weder den IT-Mode noch war das Passthrough wirklich erfolgreich.
Allerdings bietet Truenas Scale auch eine auf KVM basierte Virtualisierung und k8s basierte Container-Apps. Also weg mit Proxmox und Truenas direkt installiert, glücklicherweise bot das Supermicro-Board genug SATA Ports um auf den Controller verzichten zu können
