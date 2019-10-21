#### Jérémy ABITBOL
# TP 7 - Boot, services et processus / Tâches d’administration

## Exercice 1. Personnalisation de GRUB 

###  *1. Commencez par changer l’extension du fichier /etc/default/grub.d/50-curtin-settings.cfg s’il est présent dans votre environnement (vous pouvez aussi commenter son contenu).*
*GRUB se configure via un fichier de paramètres (/etc/default/grub), mais aussi par des scripts situés dans /etc/grub.d; ces scripts commencent tous par un numéro et sont traités dans l’ordre. 
Evidemment, seuls les scripts exécutables sont pris en compte.  
Sous Ubuntu Server, GRUB prend aussi en compte les fichiers d’extension .cfg présents dans /etc/default/grub.d. En particulier, sur les versions récentes, le fichier de configuration 50-curtin-settings.cfgdonneàlavariableGRUB_TERMINALlavaleurconsole,cequidésactive tous les paramètres liés aux fonds d’écran, thèmes, certaines résolutions, etc.*

Vu que j'ai désinstaller cloud init je n'ai pas le fichier.

### *2. Modifiez le fichier /etc/default/grub pour que le menu de GRUB s’aﬀiche pendant 10 secondes; passé ce délai, le premier OS du menu doit être lancé automatiquement.*

Il faut faire la commande suivante :
>sudo nano /etc/default/grub 

A l'intérieur du fichier il faut modifier la variable **GRUB_TIMEOUT**. On modifie la valeur de 0 à 10 ce qui permet une attente de 10 secondes avant de boot sur l’entrée par défaut.

### *3. Lancez la commande update-grub*

*Cette commande fait appel au script grub-mkconfig qui construit le fichier de configuration ”final” de GRUB (/boot/grub/grub.cfg) à partir du fichier de paramètres et des scripts*

Fait

### *4. Redémarrez votre VM pour valider que les changements ont bien été pris en compte*

 *Pensez à lancer la commande update-grub après chaque modification de la configuration de GRUB!*

 Fait

### *5. On va augmenter la résolution de GRUB et de notre VM. Cherchez sur Internet le ou les paramètres à rajouter au fichier grub.*

Il faut faire la commande suivante :
>sudo nano /etc/default/grub 

A l'intérieur du fichier il faut modifier la variable **GRUB_GFXMODE**. On modifie la valeur de la résolution personnellement j'ai mis 1024x768.

### *6. On va à présent ajouter un fond d’écran. Il existe un paquet en proposant quelques uns:grub2-splash-images (après installation, celles-ci sont disponibles dans /usr/share/images/grub).*

Téléchargement du paquet avec la comande :
>sudo apt-get install grub2-splashimages

Pour voir la banque d'image il faut faire :
>cd /usr/share/images/grub

puis :
>ls

et enfin il faut aller dans le fichier grub :
>sudo nano /etc/default/grub

et ajouter la ligne suivante :
>GRUB_BACKGROUND=/usr/share/images/grub/Plasma-lamp.tga

enfin il faut faire :
>sudo update-grub

pour sauvegarder.

### *7. Il est également possible de configurer des thèmes. On en trouve quelques uns dans les dépôts(grub2-themes-*). Installez-en un. *

Je suis aller sur internet : https://www.gnome-look.org/p/1230882/

J'ai exécuté la commande suivante :
>sudo apt install unzip

puis :
>wget -P /tmp https://github.com/shvchk/fallout-grub-theme/raw/master/install.sh

j'ai rendu le fichier executable :
>chmod +x install.sh

je l'execute :
>./install.sh

et j'ai enfin mon petit thème fallout des familles.

### *8. Ajoutez une entrée permettant d’arrêter la machine, et une autre permettant de la redémarrer.*

Il faut effectuer la commande suivante :

>sudo nano /etc/grub.d/40_custom

et ajouter ceci dans le fichier :

<pre>
# Clavier fr
insmod keylayouts
keymap fr
</pre>

### *9. Configurer GRUB pour que le clavier soit en français*


<pre>
# une entrée permettant darrêter la machine, et une autre permettant de la redémarrer.
menuentry "Eteins toi "{
halt
}
menuentry "Redemarre "{
reboot
}
</pre>

## Exercice 2. Noyau

### *1. Commencez par installer le paquet build-essential, qui contient tous les outils nécessaires (compilateurs, bibliothèques) à la compilation de programmes en C (entre autres).*

>sudo apt install build-essential

### *2. Créez un fichier hello.c*

Fait

### *3. Créez également un fichier Makefile :*

Fait

### *4. Compilez le module à l’aide de la commande make, puis installez-le à l’aide de la commande make install.*

Compilation du module à l’aide de la commande make :
>make 
<pre>
root@serveur:/home/abitbol# make
make -C /lib/modules/5.0.0-31-generic/build M=/home/abitbol modules
make[1]: Entering directory '/usr/src/linux-headers-5.0.0-31-generic'
  Building modules, stage 2.
  MODPOST 1 modules
make[1]: Leaving directory '/usr/src/linux-headers-5.0.0-31-generic'
</pre>

Installation avec la commande :
>make install
<pre>
root@serveur:/home/abitbol# make install
cp ./hello.ko /lib/modules/5.0.0-31-generic/kernel/drivers/misc
</pre>

### *5. Chargez le module; vérifiez dans le journal du noyau que le message ”La fonction init_module() est appelée” a bien été inscrit, synonyme que le module a été chargé; confirmez avec la commande lsmod.*

