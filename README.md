# HID-Arduino
Contrôler le clavier de son PC avec plusieurs types de microcontrôleurs pour mettre en place différents projets.


# I-	Arduino Micro

a)	Fonctionnement

La carte Arduino Micro est dotée de l'ATmega32u4 possédant une communication USB intégrée. Il sera donc facile de faire apparaitre la carte comme clavier sur un ordinateur.
Grace à la bibliothèque Keyboard, nous pouvons utiliser les fonctions press(), write() et releaseAll() pour contrôler le clavier de notre PC, une fois la carte connectée à celui-ci.
Keyboard.write() nous permet directement d’écrire du texte en utilisant les valeurs de la table ASCII ou en écrivant la lettre de notre choix. 
 
Pour simuler les touches du clavier, autres que les lettres, la bibliothèque utilise des macros et non des caractères ASCII.
Il est possible de presser plusieurs touches simultanément puis d’utiliser la fonction releaseAll()


b)	Projet réalisé

•	Réaliser un simple keylogger en python

•	Transformer ce programme en exe

•	Placer et exécuter ce programme dans un ordinateur grâce la carte



Dans le programme python :

On utilise la bibliothèque Pynput afin de pouvoir récupérer chaque touche pressée du clavier que l’on place ensuite dans une liste.
Lorsqu’une touche comme Backspace est appuyée, il faut pouvoir retirer la dernière lettre enregistrée. Comme c’est le cas pour tout autre touche qui ne sont pas des lettres, il faut également retirer le mot Backspace qui a été rajouté à la liste.
Pour la touche Enter, lorsque celle-ci est pressée, la liste est enregistrée dans un fichier. Enfin, un compteur s’incrémente également lorsque l’on appuie dessus. Arrivé à 5, le programme envoie par mail le fichier texte.

Mise en place du fichier EXE :

Pour transformer le .py en .exe, nous installons pyinstaller à l’aide de pip dans le dossier script. Dans ce même emplacement, nous pouvons placer une icône en .ico
Exécuter la commande suivante dans le cmd :
Pyinstaller –onefile -w -i shield.ico main.py
On obtient notre exécutable avec l’icône correspondante.

Placer le programme dans l’ordinateur :

Grace à notre HID nous pouvons placer notre programme python dans une clé qu’ira automatiquement et rapidement chercher notre programme Arduino pour le placer dans le repertoire C:\ puis dans les programmes à exécuter au démarrage.
Nous avons mis en moins de 10 secondes un programme qui enregistrera et nous enverra ce que tape un individu et qui se relancera à chaque redémarrage

# II-	Arduino UNO:

L’Arduino UNO est doté de l'ATmega328p et ne possède donc pas de module USB intégré comme la micro. Il est toutefois possible de transformer cette carte en mettant à jour le firmware de la puce avec FLIP.

Etapes : 

•	Connecter la carte Arduino au PC 


•	court-circuiter les 2 broches ICSP2 pour mettre l'Arduino en mode DFU (Device Firmware Update), afin que FLIP puisse accéder au Firmware. Les pins ICSP2 représentent une des méthodes pour programmer les cartes Arduino. On entend que l’Arduino se déconnecte du PC.


•	Cliquer sur icone USB dans FLIP et faire OPEN


•	Cliquer sur LOAD HEX FILE et choisir le fichier HEX du clavier HID


•	Cliquer sur RUN

•	Pour revenir à la normal, faire la même manipulation mais choisir le fichier HEX Arduino
 
Dans le code, nous utilisions un buffer qui contiendra les bits à envoyer au PC et qui correspondent aux touches du clavier.
Exemple : appuyer sur la touche Windows du clavier

buf[2] = 227;   //227 est le Keycode de Windows

Serial.write(buf, 8); // On envoie le registre de bits à l’ordinateur

releaseKey();  // on remet le buffer à 0

Exemple : Appuyer sur CTRL +A

buf[2] = 224;   // Ctrl

buf[4] = 20;    //A

Serial.write(buf, 8); // Send keypress

releaseKey();


# III-	ESP-32

a)	Fonctionnement

Cette fois-ci, on cherche à contrôler notre PC mais par l’intermédiaire du Bluetooth.
La bibliothèque utilisée est BLEKeyboard. Celle-ci a le même mode de fonctionnement que la bibliothèque Keyboard vue précédemment (table ASCII et mêmes macros) ainsi que les mêmes fonctions principales.
Au lieu de connecter la carte avec l’ordinateur par le port USB, nous appairerons les deux par Bluetooth dans les paramètres du PC, une fois l’esp32 allumé. Le nom de l’appareil détecté devrait etre « BLE keyboard ».
Le programme qui permet le contrôle du clavier ne s’exécutera qu’une fois que le module Bluetooth se sera connecté à un ordinateur.

b)	Projet réalisé

Un projet simple et rapide est de faire un programme qui éteint très rapidement un ordinateur connecté à l’esp32. A chaque fois que cet ordinateur activera son Bluetooth et que l’esp32 sera à proximité, allumé également, alors ils s’appaireront automatiquement et la carte exécutera son programme.
Il suffit de presser en boucle la touche ALT +F4 puis de cliquer ENTER et enfin de relâcher. Cela fermera les pages jusqu’à arriver sur le bureau ce qui proposera d’éteindre le PC. On peut également utiliser la commande shutdown.



