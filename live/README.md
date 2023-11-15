
# Hey! 🤗

# Contents

Ableton Live 11 scenes controlled by DLight via OSC or,

Ableton Live 11 scenes controlled by QLab via OSC or,

DLight being controled by Ableton Live 11 via OSC

all this using LiveGrabber https://www.showsync.com/tools

**dlight osc Project.zip** --> le .zip contient une template live et un .sho linkés en osc en localhost 127.0.0.1:7331


# 1 Ableton Live being controlled

# How to

mode d'emploi de LiveGrabber :

https://support.showsync.com/freetools/#/livegrabber/introduction

objet max for live soit GrabberReciever ( support.showsync.com/freetools/#/livegra...n?id=grabberreceiver ) et SceneGrabber ( support.showsync.com/freetools/#/livegra...tion?id=scenegrabber )

il faut juste comprendre comment envoyer un message osc depuis dlight et ajouter les deux objets dans ton projet live.

parfois il se peut que le port soit déjà occupé et que la connexion ne passe pas. alors il faut changer le numéro du port dans DL et dans l'objet GrabberReciever (53111 par exemple)

# donc, dans LIVE :

- ajouter les deux objets dans ton master
- renseigner le port si tu veux pas laisser le port par défaut (7331)
- choisir si tu veux que SceneGrabber parse par nom ou numéro (je conseil nom car numéro c'est vite galère et plus du tout gérable sous live 11)

# puis dans DLIGHT :

pour envoyer un message osc depuis dlight, il faut configurer l'adresse à laquelle tu veux l'envoyer. ça se passe ici :

- menu Display > Setup > OSC
- descends jusqu'à Output Send
- renseigner l'adresse de l'appareil où tu utilises live. si c'est le même ordi c'est 127.0.0.1 soit localhost
- renseigner le port (par défaut c'est 7331 - en local ça fonctionne, en réseau pas toujours alors j'utilise 53111)
- cliquer sur l'icône V verte

maintenant tu peux créer un S-Link dans un step

- choisir le bon step
- double cliquer sur le . sous S-infini (8 couché sur le côté)
- icone database +
- choisir SEND puis OSC puis l'adresse:port renseignée plus tôt plus haut
- renseigner ton message OSC

soit

`/Scene @s nom de la scene`

ou

`/Scene @f numéro de la scène`

ou a essayer sous live 11 ya plus de décimal pour les numéro de scène

`/Scene @i numéro de la scène`

voilà, tu peux tester en appuyant sur SEND, voir si ça marche.

tu devrais voir le message OSC dans l'objet GrabberReceiver si tout fonctionne (si besoin de debug), et ça devrait aussi lancer la scène voulue au passage.


# si QLAB est de la partie c'est :
- Settings > Network
- Name (nom du patch de sortie réseau) OSC Message UDP
- choisir l'interface réseau (automatic si en local)
- adresse ip dans destination (localhost) - port dans port (7331)

puis dans la conduite :
- créer une Network Cue
- Settings > Sélectionner le patch
- message OSC : `/Scene "nom de la scene"` (avec les "" pour éviter les ennuis) ou numéro


# 2 Ableton Live controlling DLight

En utlisant GrabberSender et TrackGrabber, ont va pouvoir lancer une CUE dans DLight depuis Ableton Live simplement en nommant un clip Midi au numéro correspondant à la Cue (ou l'ID selon le message OSC)

utiliser la template dlight osc (send and recieve).als 


# dans LIVE

- ajouter GrabberSender à la piste master du projet
- configurer l'adresse IP et le port de destination (la destination ici c'est DLight) dans GrabberSender (127.0.0.1 et 7000 dans notre template)

- créer une nouvelle track MIDI
- ajouter TrackGrabber à cette track
- dans TrackGrabber, configuer le paramètre de la track que l'on veut envoyer à DLight. pour nous ici c'est le nom du clip midi soit TriggeredClipName
- configurer le message OSC pour piloter DLight. pour lancer une Cue : `/seq/X2LoadAndFireCue` pour lancer un ID : `/seq/X2LoadAndFireID`

du coup en gros si le clip se nome 1.25, en le lançant ca va envoyer `/seq/X2LoadAndFireCue 1.25` à DLight


# dans DLIGHT

dans le setup OSC, renseiger le Port d'écoute correspondant à celui renseigné dans GrabberSender (ici 7000) et lancer la serveur OSC si ce n'est pas encore fait (bouton rouge/vert Off/On)



voilà
