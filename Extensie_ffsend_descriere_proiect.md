# SendBridge – extensie de browser pentru partajarea securizată a fișierelor prin ffsend

## 1. Descriere scurtă

SendBridge este o extensie de browser (Chrome/Chromium și Firefox) care permite trimiterea rapidă și sigură a fișierelor printr-un server Send, folosind utilitarul în linie de comandă **ffsend**. Utilizatorul alege un fișier din browser, extensia îl transmite unui program local care rulează ffsend, iar fișierul este criptat pe calculatorul utilizatorului înainte de a fi încărcat. Rezultatul este un link securizat, cu limită de descărcări și termen de expirare, copiat automat în clipboard.

## 2. Descriere detaliată

Serviciul Send (inițial Firefox Send, astăzi continuat ca proiect open-source întreținut de comunitate) permite partajarea fișierelor cu criptare end-to-end: fișierul este criptat în dispozitivul expeditorului, iar cheia de decriptare face parte din link și nu ajunge niciodată pe server. ffsend este clientul în linie de comandă pentru acest serviciu, foarte util în scripturi, dar incomod pentru utilizatorii obișnuiți, care nu lucrează în terminal.

SendBridge face legătura dintre browser și ffsend. Extensiile de browser nu pot rula direct programe instalate în sistem, așa că proiectul folosește mecanismul **Native Messaging**: extensia comunică printr-un canal standardizat (mesaje JSON prin intrarea și ieșirea standard) cu un mic program gazdă instalat local, scris în Python. Acesta primește cererile extensiei, apelează ffsend cu parametrii potriviți și returnează rezultatul.

### Arhitectura sistemului

Sistemul are patru componente:

1. **Extensia de browser** – interfața cu utilizatorul: fereastra popup din bara de instrumente, meniul contextual (click dreapta) și pagina de setări.
2. **Programul gazdă Native Messaging** (Python) – validează cererile, construiește comanda ffsend, o execută și transmite înapoi rezultatul și progresul.
3. **ffsend** (aplicație Rust) – realizează criptarea, încărcarea, descărcarea și gestionarea fișierelor pe serverul Send.
4. **Serverul Send** – o instanță publică sau una proprie (self-hosted), care stochează temporar fișierele criptate.

### Fluxul de funcționare

1. Utilizatorul deschide popup-ul extensiei și selectează un fișier, sau alege din meniul contextual opțiunea „Trimite cu Send” pentru un fișier descărcat recent.
2. Utilizatorul setează opțiunile: numărul maxim de descărcări, durata de valabilitate și, opțional, o parolă.
3. Extensia trimite programului gazdă o cerere JSON cu calea fișierului și opțiunile alese.
4. Programul gazdă rulează `ffsend upload` cu parametrii corespunzători; fișierul este criptat local și apoi încărcat.
5. Linkul generat este returnat extensiei, afișat utilizatorului și copiat în clipboard.
6. Fișierul este adăugat în istoricul local, de unde poate fi ulterior verificat (`ffsend info`) sau șters de pe server (`ffsend delete`).

### Funcționalități principale

- încărcarea fișierelor și a directoarelor (arhivate automat de ffsend);
- configurarea limitei de descărcări și a timpului de expirare;
- protejarea opțională cu parolă;
- copierea automată a linkului în clipboard și afișarea unui cod QR;
- istoric al fișierelor trimise, cu posibilitatea de a vedea starea și de a le șterge;
- descărcarea și decriptarea fișierelor primite pe baza unui link Send;
- alegerea serverului Send folosit (instanță publică sau server propriu);
- notificări la finalizarea încărcării sau în caz de eroare.

## 3. Tehnologii folosite

JavaScript; HTML5; CSS3; WebExtensions API; Manifest V3; Native Messaging; Python 3; JSON; ffsend; Rust; Send; Node.js; AES-GCM; HKDF; HTTPS; Git; Visual Studio Code; web-ext; Chrome DevTools; Chrome/Chromium; Microsoft Edge; Firefox; Windows; Linux; macOS

## 4. Scop și domeniu

### Scop

Scopul proiectului este de a face partajarea securizată a fișierelor la fel de simplă ca un click în browser, păstrând în același timp avantajele de securitate ale ffsend: criptare locală, linkuri cu durată limitată și lipsa accesului serverului la conținutul fișierelor. Proiectul oferă o alternativă respectuoasă față de confidențialitate la serviciile clasice de transfer de fișiere, unde furnizorul poate vedea datele încărcate.

### Domeniu

Proiectul se încadrează în domeniul **securității informației și al protejării datelor personale**, cu aplicare concretă în **partajarea securizată a fișierelor**. Totodată, atinge domeniile **dezvoltării de extensii pentru browsere** și al **integrării aplicațiilor web cu aplicații native din sistemul de operare**.

Utilizatorii vizați sunt persoanele și echipele care trimit frecvent documente sensibile (contracte, acte, fișiere de lucru), studenții și profesioniștii IT care vor o alternativă open-source, precum și organizațiile care își pot găzdui propriul server Send.

## 5. Obiective

### Obiectiv general

Realizarea unei extensii de browser funcționale care integrează clientul ffsend și permite trimiterea, gestionarea și primirea fișierelor criptate end-to-end direct din browser, fără folosirea terminalului.

### Obiective specifice

1. Proiectarea arhitecturii de comunicare dintre extensie și sistemul de operare prin Native Messaging.
2. Implementarea programului gazdă în Python, care execută în siguranță comenzile ffsend și validează toate datele primite de la extensie.
3. Dezvoltarea unei interfețe simple și intuitive (popup, meniu contextual, pagină de setări).
4. Implementarea opțiunilor de securitate: limită de descărcări, expirare, parolă.
5. Realizarea unui istoric local al fișierelor trimise, cu verificarea stării și ștergerea de pe server.
6. Asigurarea compatibilității cu Chrome/Chromium și Firefox, pe Windows, Linux și macOS.
7. Posibilitatea folosirii unui server Send propriu, pentru control complet asupra datelor.
8. Testarea funcțională și de securitate a aplicației și documentarea procesului de instalare și utilizare.

## 6. Rezultate așteptate

La finalul proiectului va exista o extensie instalabilă, împreună cu un program gazdă și un script de instalare, care permit oricărui utilizator să trimită fișiere criptate în câteva secunde, direct din browser. Proiectul demonstrează practic cum pot fi combinate tehnologiile web cu instrumentele native ale sistemului de operare pentru a oferi funcționalități sigure și ușor de folosit.
