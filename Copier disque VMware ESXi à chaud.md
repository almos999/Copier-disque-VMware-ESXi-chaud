
# Introduction 

Nous devons copier le disque qui contient l'*Active Directory* du campus à chaud pour assurer la continuité de l'environnement de l'AD.

Il est important de copier des disques *VMware ESXi* à chaud lorsque l'environnement inclut un contrôleur de domaine Active Directory, car cela permet de garantir la continuité de service et l'intégrité des données. En effet, interrompre le fonctionnement d'un contrôleur de domaine *Active Directory* peut causer des problèmes importants pour les utilisateurs et les applications en dépendant de ces services. La copie à chaud permet de créer une copie exacte des disques sans interruption de service, ce qui est crucial pour maintenir la disponibilité des services *Active Directory*. Il est donc essentiel de planifier soigneusement la copie à chaud des disques pour minimiser les risques et assurer la continuité opérationnelle de l'environnement *Active Directory*.

Avant de copier un disque VMware il est important de savoir qu'il va falloir supprimer toute les *snapchot* pour consolider le disque et recréer une *"snapchot"*. (Il est donc important de noter que la suppression de snapshots peut entraîner une utilisation importante des ressources système et un temps de traitement important.).
Une instantanée de disque, ou "snapshot" en anglais, est une copie instantanée des données présentes sur un disque à un moment donné. Cette copie peut être utilisée pour sauvegarder les données ou pour restaurer le disque à un état précédent en cas de problème.


# 1] Snapchot remove

Pour copier le disque a chaud que vous voulez vous devrez vous rendre sur votre serveur à l'aide de son IP, exemple :

	- https://example/ui

(Si vous n'avez pas de snapchot vous pouvez passer l'étape 1)

Vous devez vous rendre sur la machine virtuelle et supprimer toutes les snapchots en faisant clique droit la machine virtuelle **'Snapchots>Gérer les snapchots>'**, cliquez sur **'Supprimer tout'**, **'Supprimer'**.

<img src="https://cdn.discordapp.com/attachments/1029113801003511859/1062654909243195392/image.png">

Un message d'erreur est apparu indiquant que les snapshots seront consolidées en un seul disque et qu'elles seront supprimées. À partir de maintenant, votre machine virtuelle écrira sur le disque principal et non sur la snapshot (ce qui mettra à jour votre disque).

# 2] Snapchot add

Pour copier la version la plus récente du disque dur virtuel, il est important de consolider les snapshots et de créer une nouvelle snapshot. Cela permet d'écrire sur la snapshot plutôt que sur le disque principal, ce qui évite de rendre celui-ci non disponible pour une copie. En créant une snapshot, on libère ainsi l'utilisation du disque principal. **Ce qu'il faut noter, c'est que tout ce qui sera écrit sur la snapshot que l'on créera ne sera pas copié.** Si vous voulez copier toutes les données sans exception, il faudra éteindre la machine virtuelle.

Pour se faire vous devez vous rendre sur la machine virtuelle, clique droit **'Snapchots>Prendre un snapchot>'**, donnez lui un nom et cliqué sur **'Prendre un snapchot'** 

<img src="https://cdn.discordapp.com/attachments/1029113801003511859/1062680739063271465/image.png">

A partir de là, si vous regardez vos snapshots, vous verrez que vous avez celle que vous avez créé.

<img src="https://cdn.discordapp.com/attachments/1029113801003511859/1062682205723316244/image.png">

# 3] Copie du disque

Ici, nous allons voir comment copier le disque dans un autre dossier en local sur VMware ESXi pour pouvoir le télécharger. Pour cela, vous devez faire un clic droit sur **Stockage**, puis sélectionner **'Stockage > Parcourir les banques de données'**. Vous vous rendrez dans le dossier où se trouve votre disque, puis vous ferez un clic droit dessus pour sélectionner **'Copier'**

<img src="https://cdn.discordapp.com/attachments/1029113801003511859/1062687887612706876/image.png">

Ensuite, il faudra sélectionner le chemin où vous voulez la coller. Dans mon cas, je la mettrais à la racine de "ssd"

<img src="https://cdn.discordapp.com/attachments/1029113801003511859/1062690331549773894/image.png">

# 4] Téléchargement du disque

Maintenant, nous allons voir comment télécharger le disque virtuel. Pour cela, vous aurez juste à vous rendre dans l'Explorateur de banque de données, sélectionner votre disque et cliquer sur "Télécharger".

<img src="https://cdn.discordapp.com/attachments/1029113801003511859/1062691156703596604/image.png">

<img src="https://cdn.discordapp.com/attachments/1029113801003511859/1062691266787282994/image.png">

# 5] Migration du disque VMDK vers VHD

Pour convertir le disque il faudra le logiciel **[Vmdk2Vhd](https://www.softpedia.com/get/System/File-Management/Vmdk2Vhd.shtml)**

Une fois le logiciel installé, vous le lancez et vous devrez sélectionner le disque source (disque VMware) et la destination où votre disque Hyper-V converti sera enregistré. Après, cliquez sur *'Convert'* pour convertir le disque.

<img src="https://cdn.discordapp.com/attachments/1029113801003511859/1062290887390003261/image.png">
