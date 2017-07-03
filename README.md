# PoC Video

https://www.youtube.com/watch?v=dhTAqCz7ktU

# Man in The Middle

In computer security, a man-in-the-middle attack (MITM) is an attack where the attacker secretly relays and possibly alters the communication between two parties who believe they are directly communicating with each other. {Wikipedia}

Il sito internet ufficiale di VLC (videolan.org) nonostante sia dotato di un certificato SSL valido non veicola obbligatoriamente gli utenti ad utilizzare il protocollo HTTPS. Anche i principali motori di ricerca (Google, Bing, Yahoo) hanno indicizzato il sito senza HTTPS veicolando gli utenti sulla versione non sicura del sito internet.

Secondo le stime pubblicate dal fondatore di VLC il software è stato scariato oltre 2,3 miliardi di volte: https://twitter.com/etixxx/status/881926077836398592

Tramite un attacco Man in The Middle è possibile pertanto iniettare codice HTML arbitrario nel sito ufficiale di VLC, in versione non HTTPS, illudendo l'utente di scaricare un file eseguibile di installazione autentico ma realmente i link originali sono stati alterati per indirizzare l'utente ad una risorsa esterna che potrebbe contenere Malware. L'attacco MiM è possibile generarlo quando la vittima e l'attaccante si trovano nella stessa rete, un Access Point Wireless di un Aeroporto, Albergo e Bar sono tre comuni esempi.

Il risultato dell'attacco è un alterazione della pagina di download (es. http://get.videolan.org/vlc/2.2.6/win32/vlc-2.2.6-win32.exe) nella quale andremo ad alterare le seguenti porzioni di codice:

<meta http-equiv="refresh" content="5;URL='https://videolan.mirror.garr.it/mirrors/videolan/vlc/2.2.6/win32/vlc-2.2.6-win32.exe'" />

<span> If not, <a href="https://videolan.mirror.garr.it/mirrors/videolan/vlc/2.2.6/win32/vlc-2.2.6-win32.exe" id="alt_link">click here</a>.


# How To

Per effettuare l'attacco sfruttiamo il software Bettercap (https://www.bettercap.org/) e un modulo Ruby scritto ad-hoc "exploit_vlc.rb" pubblicato anche esso in questa repository.

All'interno del modulo personalizzato andremo a indicare il nuovo mirror del file eseguibile che vogliamo far scaricare alla vittima.

Terminata questa personalizzazione avviamo Bettercap con i permessi di root, digitando da terminale:

$ sudo bettercap --proxy-module exploit_vlc.rb

Eventualmente è possibile indicare uno specifico target con il parametro -T e l'interfaccia di rete con il paramentro -I. Se non viene specificato un target tutti gli host collegati alla rete potrebbero essere dei possibili target.

# Disclaimer 

La tecnica illustrata è puramente a scopo divulgativo, può inoltre essere sfruttata su qualsiasi sito internet che diffonde software. L'attenzione nostra è ricaduta su VLC poichè è un software di grande interesse pubblico come ci ha anche affermato lo sviluppatore principale.
