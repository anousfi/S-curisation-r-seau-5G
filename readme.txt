
+--------------------------------------------------------------+

,--.       ,-----.,--.  ,--.  ,---.  ,--.   ,--. 
|  |,---. '  .--./|  ,'.|  | /  O  \ |   `.'   | 
|  | .-. :|  |    |  |' '  ||  .-.  ||  |'.'|  | 
|  \   --.'  '--'\|  | `   ||  | |  ||  |   |  | 
`--'`----' `-----'`--'  `--'`--' `--'`--'   `--' 
                                                 
		RSX218 - Semestre 2 - 2024/2025
				Stefano Secci

	Projet n° 4 : Sécurisation des échanges
			dans un cœur 5G SA
	
		Encadrant : Christian Destré


			   Etudiants :
			Alexandre Moniot
		  Mounir Mounpain Ndam
			 Yali N’Gbe-Baï
			   Anas Nousfi

+--------------------------------------------------------------+



Ce zip contients les fichiers principaux de configuration des coeurs 5G Free 5GC et Open5GS et du simulateur UERANSIM.
Il contient également un fichier de résultats complet d'un scann TLS-Scanner de la NRF pour chacun des deux projets Open5GS et Free5GC

Les informations de ce readme concerne l'utilisation du simulateur UERANSIM et des deux coeurs 5G Free5GC et Open5GS dans le projet. 
Les coeurs ont été déployé via Docker, il faut donc que Docker et Docker Compose soient installés sur la machine hôte.
Voir ici pour l'installation de Docker : https://docs.docker.com/engine/install/

Les fichiers de configurations proposés sont fait pour faire fonctionner le coeur sur une VM et le simulateur sur une seconde machine, ainsi les fichiers compose exposent certains ports des conteneurs.

Les fichiers sont quasiment fonctionnels en l'état, a l'exception de certains paramètres tels que l'adresse IP des machines hotes à ajuster dans certains fichiers de configs.
De plus il faut ajouter les utilisateurs à la base d'abonnés des coeurs.




#### Versions ####

Au moment de la réalisation de ce projet, les versions utilisées sont les suivantes : 
	* Docker : 28.0.4
	* Docker compose : 2.34.0
	* UERANSIM : 3.2.7
	* Free5GC : 4.0.1
	* Open5GS : 2.7.5





#### Description des fichiers ####

Les fichiers de cette archive sont les suivants : 


Livrabble-FichiersDeConf
├── Fichiers de configurations					
│		 ├── Free5GC
│		 │		 ├── config								<===# Dossier contenant les Fichiers de configuration TLS désactive de Free5GC (amfcfg.yaml etc...)
│		 │		 ├── config-TLS							<===# Dossier contenant les Fichiers de configuration TLS activé de Free5GC (amfcfg.yaml etc...)
│		 │		 ├── docker-compose-tls.yaml			<===# Fichier Compose permettant de lancer le coeur avec TLS actif
│		 │		 └── docker-compose.yaml				<===# Fichier Compose permettant de lancer le coeur avec TLS désactivé
│		 ├── Open5GS
│		 │		 ├── AddSubscribers_O5GS.py				<===# Script python permettant d'ajouter les subsribers à la base Open5GS
│		 		 ├── compose-files
│		 │		 │	 ├── basic					
│		 │		 │		     └── docker-compose.yaml	<===# Fichier Compose permettant de lancer le coeur avec TLS désactivé
│		 │		 │	 └── basictls
│		 │		 │		     └── docker-compose.yaml	<===# Fichier Compose permettant de lancer le coeur avec TLS actif
│		 │		 └── fichiers config
│		 │		     ├── basic							<===# Dossier contenant les Fichiers de configuration noTLS des NF Open5GS (amf.yaml etc...)
│		 │		     └── basictls						<===# Dossier contenant les Fichiers de configuration TLS des NF Open5GS (amf.yaml etc...)
│		 └── UERANSIM
│		     ├── free5gc-gnb.yaml						<===# Fichier de config du GNB pour Free5GC
│		     ├── free5gc-ue-nopdu.yaml					<===# Fichier de config de l'UE (sans PDU establisheent) pour Free5GC
│		     ├── free5gc-ue.yaml						<===# Fichier de config de l'UE pour Free5GC
│		     ├── open5gsdocker-gnb.yaml					<===# Fichier de config du GNB pour Open5GS
│		     ├── open5gs-ue-nopdu.yaml					<===# Fichier de config de l'UE (sans PDU establisheent) pour Open5GS
│		     └── open5gs-ue.yaml						<===# Fichier de config de l'UE pour Open5GS
├── Résultats TLS-Scanner					
│		 ├── Free5GC_Scan TLS-Scanner_NRF.txt			<===# Résultat complet du scann TLS de la NRF Free5GC
│		 └── Open5GS_Scan TLS-Scanner_NRF.txt			<===# Résultat complet du scann TLS de la NRF Open5GS
└── readme.txt											<===# Ce readme





#### UERANSIM ####
UERANSIM est disponible sur ce dépot : https://github.com/aligungr/UERANSIM

Après avoir configurer les fichiers de confs (adresses IP), lancer les commandes suivantes : 
	* Démarrage et connexion du GNB :
		./build/nr-gnb -c config/FichierConfigGNB.yaml
			exmeple pour Open5GS : ./build/nr-gnb -c config/open5gsdocker-gnb.yaml
	
	* Démarrage et connexion d'un UE :
		./build/nr-ue -c config/FichierConfigUE.yaml
			exemple pour Free5GC : ./build/nr-gnb -c config/free5gc-ue.yaml
			
	* Démarrage et connexion de plusieurs UE :
		./build/nr-ue -c config/FichierConfigUE.yaml -n xxx #xxx = nombre d'UE
			exemple de 100 UE pour Free5GC : ./build/nr-gnb -c config/free5gc-ue.yaml -n 100
		
		
#### Free5GC ####
La source utilisée dans notre projet est le dépot suivant : https://github.com/free5gc/free5gc-compose

Il faut cloner le dépot sur la machine via : git clone https://github.com/free5gc/free5gc-compose.git
Remplacer ensuite les différents fichiers de configuration et compose

Les commandes pour lancer le coeur Open5GS sont les suivantes : 
 * TLS off : docker compose -f docker-compose.yaml up -d
 * TLS on  : docker compose -f docker-compose-tls.yaml up -d
 
Pour détruire les conteneurs : 
 * TLS off : docker compose -f compose-files/basic/docker-compose.yaml --env-file=.env down
 * TLS on  : docker compose -f compose-files/basictls/docker-compose.yaml --env-file=.env down


#### Open5GS ####
La source utilisée dans notre projet est le dépot suivant : https://github.com/Borjis131/docker-open5gs

Il faut cloner le dépot sur la machine via : git clone https://github.com/Borjis131/docker-open5gs.git
Remplacer ensuite les différents fichiers de configuration et compose

Les commandes pour lancer le coeur Open5GS sont les suivantes : 
 * TLS off : docker compose -f compose-files/basic/docker-compose.yaml --env-file=.env up -d
 * TLS on  : docker compose -f compose-files/basictls/docker-compose.yaml --env-file=.env up -d
 
Pour détruire les conteneurs : 
 * TLS off : docker compose -f compose-files/basic/docker-compose.yaml --env-file=.env down
 * TLS on  : docker compose -f compose-files/basictls/docker-compose.yaml --env-file=.env down


#### SSLyze ####
La source utilisée dans notre projet est le dépot suivant : https://github.com/nabla-c0d3/sslyze




#### TLS_Scanner ####
La source utilisée dans notre projet est le dépot suivant : https://github.com/tls-attacker/TLS-Scanner