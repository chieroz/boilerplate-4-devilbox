# Devilbox by Chieroz

## concetto generale

l'idea è quella di avere una cartella per "server" ovvero per tutti i progetti che condividono lo stesso server o quanto meno le stesse versioni di PHP, MySQL o MariaDB, eccetera.

## procedure

il grosso del lavoro lo farai da linea di comando:per prima cosa devi creare la struttura delle cartelle che conterranno il tuo set.

    cd ~/Sites
    mkdir [NOME_PROGETTO]
    cd [NOME_PROGETTO]
    mkdir backups
    mkdir projects

Ora devi clonare Devilbox (sono circa 150 Mb). Io lo metto in una cartella con nome "devilbox[NOME_PROGETTO]" perché ti permette di trovare rapidamente i volumi dentro Docker.

    git clone https://github.com/cytopia/devilbox devilbox_[NOME_PROGETTO]

Ti rimane da creare il file .env copiandolo dal file di esempio che trovi nella cartella di Devilbox:

    cp env-example .env

Scarica questi due gist da Github:

- env-constants [link](https://gist.github.com/chieroz/8711716d9fe1a68db21a6b37c3f18aeb)
- docker-compose-override.yml [link](https://gist.github.com/chieroz/5514cc58bb812fcc0ba7b6000b5d212b)

Il primo contiene le costanti che devi andare a modificare nel file .env che hai creato. In questo stesso file devi andare a indicare che versione di PHP, Mysql eccetera vuoi usare.

Il secondo file copialo direttamente nella cartella di Devilbox. Occhio: per ogni cartella che contiene un'installazione di Devilbox devi cambiare le maschere di rete altrimenti Docker si incazza.

## Avviare, stoppare eccetera

Il comando per avviare Docker è:

    docker compose up -d [lista dei container]

Io per esempio uso questa:

    docker compose up -d bind httpd mysql php

Se lanci senza lista dei container li fa partire tutti e forse è meglio evitare...

Per fermare ovviamente:

    docker compose stop

Io ogni volta stoppo ed elimino tutto:

    docker compose stop; docker compose rm -f

## Prima installazione e aggiornamenti

Ovviamente la prima volta devi lanciare:

    docker compose pull

Le immagini vengono aggiornate abbastanza frequentemente per cui io ogni tanto lancio nuovamente il comando per scaricare le nuove versioni.

## Personalizzazioni

Nella cartella **projects** devi andare a mettere i progetti che vuoi sviluppare. Io per esempio tengo tutti i progetti EDT in una stessa "installazione" di Devilbox perché stanno tutti sullo stesso server di produzione o comunque stanno su server tutti uguali.

Se non vuoi che il browser ti rompa le scatole con il certificato, puoi importarlo nel browser (per Firefox) oppure nei Keychain (per Chrome). Il certificato sta nella cartella **ca**.

La cartella **autostart** può ospitare degli script che vuoi lanciare ogni volta che fai "docker composer up". Io per esempio ho aggiunto uno script git-config-global.sh che imposta le variabili di git in modo da poter operare da dentro il container senza dover ogni volta inserire i dati.

```
#!/usr/bin/env bash

su -c "git config --global user.name \"NAME\"" -l devilbox
su -c "git config --global user.email \"NAME@DOMAIN.TLD\"" -l devilbox
su -c "git config --global init.defaultBranch main" -l devilbox
```

Nella cartella **cfg** trovi una serie infinita di sottocartelle, ognuna delle quali contiene dei file di esempio. Una volta capito come funzionano, per utilizzarli (magari per personalizzare MySQL o avviare l'installazione di un modulo aggiuntivo di PHP) devi semplicemente togliere "-example" dal nome del file (ovviamente dopo aver personalizzato il file con le impostazioni che ti servono).

Come inizio direi che può bastare.
