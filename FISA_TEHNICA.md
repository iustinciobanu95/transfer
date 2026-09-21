# Fișă tehnică — Aplicație Registratură Electronică / Management Documente (Alfresco)

> Sistem de registratură electronică și management de documente pentru unități militare, construit peste platforma ECM **Alfresco**. Backend Laravel (API REST) + frontend Vue/Vuetify (SPA).

---

## 1. Scop și funcționalități

### Scop
Digitalizarea circuitului documentelor la nivelul unităților militare: **înregistrare (registratură), clasificare, stocare, regăsire și raportare** a documentelor, cu păstrarea fișierelor și a metadatelor în repository-ul **Alfresco**. Aplicația funcționează ca strat de orchestrare între utilizatori, baza de date relațională (numerotare/registre) și una sau mai multe instanțe Alfresco (documente + aspecte/metadate).

### Rol în ecosistem
- **Front-office de registratură** peste Alfresco: alocare numere de intrare/ieșire, tipuri de documente, stări, locații.
- **Punte (integration layer)** între aplicații interne (aplicația „Personal”, Kestra, Redmine, ERP angajați) și Alfresco DMS.
- **Generator de registre și rapoarte PDF** (clasificate / neclasificate).

---

## 2. Funcționalități principale

| Zonă | Funcționalitate |
|------|-----------------|
| **Registratură documente** | Încărcare document de secretariat, document electronic, document pe suport de stocare (storage device); alocare automată număr de intrare din interval (range of numbers) |
| **Numerotare / registre** | Intervale de numere pe unitate/tip document/an; număr curent; activare interval curent; **anulare număr** (canceled numbers) și înregistrare pe număr anulat |
| **Rezoluție documente** | Înregistrarea documentelor electronice „pentru prima rezoluție”; liste per secretariat / electronice / suport de stocare |
| **Clasificare & metadate** | Gestiune „document aspects” (clasificare, expeditor/destinatar, nivel de secretizare, anexe, indicativ dosar etc.) și **sincronizare aspecte** cu nodurile Alfresco |
| **Nomenclatoare** | Tipuri documente, stări document, locații Alfresco, expeditor/destinatar intern & extern, indicative de dosar (file identifiers), locații unitate militară |
| **Unități militare** | CRUD unități, configurare foldere Alfresco (site/base folder), import utilizatori din Alfresco, generare registru |
| **Utilizatori** | Listare/creare/ștergere, filtrare pe unitate militară, import din Alfresco |
| **Preview / linkuri** | Preview conținut document și obținere linkuri Alfresco (share) |
| **Permisiuni Alfresco** | Adăugare grupuri unități militare pe document, rol „consumer”, permisiuni pe documente personale |
| **Integrări externe** | Upload document / DPS / declarație în Alfresco DMS din aplicația Personal; import CV (DMRU) în Alfresco DMS declanșat din Kestra |
| **Rapoarte** | Generare PDF registru — variantă **clasificată** și **neclasificată** (TCPDF/FPDI) |

---

## 3. Utilizatori (roluri)

Autentificarea se face pe conturi de **Active Directory** (domeniul `sts.nest`), iar autorizarea la nivel de aplicație pe tabela `users` (utilizatorul trebuie să existe local pentru a primi token).

- **Utilizator de unitate militară / secretariat** — operatorul de registratură (înregistrează, clasifică, alocă numere, generează registre). Legat de o unitate militară (`users.military_unit_id`).
- **Administrator** (`admin`) — cont local special, autentificat direct din baza de date (nu din AD), acces administrativ.
- **Cont de serviciu `alfresco.user`** — cont tehnic cu token cu durată lungă, folosit de integrări server-to-server (aplicația Personal, importuri).
- **Sisteme externe** (Personal / Kestra / ERP / Alfresco) — clienți API care apelează endpoint-urile de upload/import cu token de tip „personal”, verificat cu expirare custom.

---

## 4. Date prelucrate

Persistate în **MySQL** (numerotare, nomenclatoare, referințe spre Alfresco) + fișiere și metadate în **Alfresco**.

### Entități principale (schema DB)
- **users** — `name, military_unit_id, username, email, password, tokens`
- **military_units** — `name, short_name, code`
- **military_unit_locations** — `military_unit_id, alfresco_location_id, code, alfresco_site, alfresco_folder_base_id, alfresco_folder_base_location`
- **military_unit_documents** (entitatea centrală) — `military_unit_id, document_type_id, document_aspect_id, alfresco_node_id, alfresco_location_code, child_id, entrance_number, entrance_date, original_name, new_name, base64_to_md5, uploaded_by, uploaded_date` + flag-uri: `is_electronic_document, is_canceled_number, is_exit_document, is_electronic_document_first_resolution`
- **military_unit_range_of_numbers** — `military_unit_id, document_type_id, alfresco_location_id, year, is_current_range, used, first_number, last_number, current_number`
- **canceled_numbers** — `military_unit_id, military_unit_document_id, entrance_number`
- **document_aspects** — set bogat de metadate: clasificare, descriere, nr./dată expeditor, stare, tip, expeditor/destinatar intern & extern, anexe (clasificare/prioritate), subiect/dată/expeditor mail, adresă, nivel secretizare document/anexă, indicativ dosar etc.
- **document_types**, **alfresco_document_states**, **alfresco_locations** — nomenclatoare (`name, code`)
- **alfresco_intern_receiver_senders / alfresco_extern_receiver_senders** — expeditori/destinatari
- **intern/extern_receiver_sender_entrance_numbers** — legături număr intrare ↔ expeditor/destinatar
- **file_identifiers** — indicative de dosar (`name, short_name, year, is_active`)
- **personal_access_tokens** (Sanctum), **failed_jobs**, **password_resets**

### Date sensibile
- Documente clasificate și metadate de secretizare.
- Date cu caracter personal (CV-uri DMRU: nume, CNP, data nașterii, email, telefon, adresă, studii — vezi `CvUploadController`).
- Interogare ERP angajați după **CNP** (aplicația Personal).

---

## 5. Fluxuri de date și interconectări

### 5.1 Schema de arhitectură și interconectări

![Schema de arhitectură și interconectări](docs/diagrams/arhitectura.png)

- **Frontend ↔ Backend:** SPA servit de ruta catch-all `web.php` (`/{any} → view('application')`); toate operațiile prin `routes/api.php` cu middleware `auth:sanctum`.
- **Backend ↔ Alfresco:** prin `AlfrescoRestProvider` (API REST Alfresco, Guzzle) — upload fișiere, creare/citire noduri, foldere, aspecte (metadate), permisiuni/roluri, linkuri share, preview. Configurabil pe mai multe instanțe (`alfresco`, `alfresco-dms`).
- **Backend ↔ aplicația Personal:** endpoint-uri `/alf-dms/upload-document`, `/upload-dps`, `/upload-declaration` protejate cu `check.custom.token.expiration`.
- **Backend ↔ Kestra (orchestrare):** endpoint `/alf-dms/import-cv` (import CV DMRU), protejat cu `check.custom.token.expiration`.
- **Backend ↔ Redmine:** ștergere issue asociat unui document (`DocumentSecretariat:LinkIssueRedmine`).
- **Backend ↔ ERP Personal:** `GET https://personal.sts.ro/api/erp/employees?filter[cnp]=...` pentru date angajat.

### 5.2 Flux — înregistrare document în registratură

![Flux înregistrare document](docs/diagrams/flux-inregistrare-document.png)

### 5.3 Flux — autentificare (Active Directory + Sanctum)

![Flux autentificare](docs/diagrams/flux-autentificare.png)

### 5.4 Flux — integrare externă (upload documente & import CV)

Endpoint-urile `/alf-dms/*` sunt apelate de **aplicația Personal** (`upload-document`, `upload-dps`, `upload-declaration`) și de **Kestra** (orchestrare) pentru `/import-cv`, toate cu token cu expirare custom.

![Flux integrare externă (Personal / Kestra)](docs/diagrams/flux-integrare-externa.png)

---

## 6. Autentificare

- **Mecanism aplicație:** token **Laravel Sanctum** (Bearer), stocate în `personal_access_tokens`. Login: `POST /api/login` cu `username`, `password`, `device_name` → returnează `plainTextToken`.
- **Sursa de identitate:** **Active Directory** prin `adldap2/adldap2-laravel`. La login se caută `userPrincipalName = <username>@sts.nest`, se face `bind`/`attempt` cu parola; dacă e valid și utilizatorul există local → se emite token. Cont `admin` = excepție, validat local cu `Hash::check`.
- **Sesiuni server-to-server:** token „personal” cu abilitatea `expires_in` verificat de middleware **`CheckCustomTokenExpiration`** (expirare custom calculată din `created_at + expires_in`). Cont de serviciu `alfresco.user` cu token generat prin comanda `alfresco:generate-user-token` (validitate lungă).
- **Logout:** `POST /api/logout` — revocă token-urile utilizatorului.
- **Config AD:** `config/app.php → active_directory_connection` (hosts, base_dn, user, pass din `.env`); `config/ldap.php` (adldap2).
- **Credențiale Alfresco:** citite exclusiv din `.env` (`ALFRESCO_USER`, `ALFRESCO_PASSWORD`), fără valori hardcodate în `config/alfresco*.php`.

---

## 7. Managementul modificărilor (change management)

- **Schema DB:** migrări Laravel versionate cronologic în `database/migrations/` (nucleu funcțional creat 2022-12-05; extinderi 2023–2024: `file_identifiers`, `military_unit_locations`).
- **Sincronizare metadate:** endpoint-uri `PATCH /document-aspects/{id}/sync-{secretary|electronic|storage-device}/{alfresco_node_id}` — reconciliază aspectele din DB cu nodurile Alfresco.
- **Actualizare noduri Alfresco:** comandă artisan `UpdateAlfrescoNodes` (`app/Console/Commands`).
- **Anulare/corecție numere:** `PATCH /cancel-number/{id}` + înregistrare pe număr anulat (trasabilitate în `canceled_numbers`).
- **Trasabilitate document:** câmpuri `uploaded_by`, `uploaded_date`, `base64_to_md5` (integritate/deduplicare), `child_id` (versiune/legătură document părinte-copil), `timestamps`.
- **Versionare cod:** proiectul actual **nu este repo git** (folder `alfresco-old`); recomandat control de versiune + pipeline (există `.styleci.yml`, ESLint, Prettier pentru stil).

---

## 8. API-uri (endpoint-uri expuse de aplicație)

Toate sub prefix `/api`, majoritatea cu `auth:sanctum`. Selecție reprezentativă:

**Auth & sistem**
- `POST /login`, `POST /logout`, `GET /user`, `GET /version`

**Documente (registratură)**
- `GET /secretary-documents-list`, `GET /electronic-documents-list`, `GET /storage-device-documents-list`
- `POST /upload-secretary-document` (+ `-canceled-number`)
- `POST /upload-electronic-document` (+ `-canceled-number`)
- `POST /upload-storage-device-document` (+ `-canceled-number`)
- `POST /register-electronic-documents-for-first-resolution`
- `PATCH /cancel-number/{id}`
- `GET /documents/{alfresco_node_id}/alfresco-aspects`, `GET /preview/{alfresco_node_id}`

**Aspecte / sincronizare**
- `PATCH /document-aspects/{id}`, `.../sync-secretary|sync-electronic|sync-storage-device/{alfresco_node_id}`

**Alfresco / permisiuni**
- `GET /electronic-documents-for-first-resolution/list`, `GET /alf-dms-user-personal-documents`, `GET /alf-dms-users`
- `POST /alf-dms-add-user-personal-documents-permission`
- `PATCH /add-all-military-units-groups-to-document/{alfresco_node_id}`, `GET /document/{alfresco_node_id}/links`, `GET /add-consumer-role`

**Integrare „Personal” (token cu expirare custom)**
- `POST /alf-dms/upload-document`, `/alf-dms/upload-dps`, `/alf-dms/upload-declaration`, `/alf-dms/import-cv`

**Nomenclatoare & entități**
- `GET /document-types`, `/alfresco-locations`, `/alfresco-document-states`, `/alfresco-intern-receiver-sender`, `/alfresco-extern-receiver-sender`
- `.../military-units`, `/military-unit-locations`, `/military-unit-range-of-numbers` (CRUD + `activate`), `/file-identifiers` (CRUD), `/users` (list/create/delete/import-alfresco), `POST /generate-register`

---

## 9. Tehnologii folosite

**Backend**
- **PHP ^7.2**, **Laravel 7.24**
- **Laravel Sanctum** (autentificare API prin token)
- **adldap2/adldap2-laravel** (integrare Active Directory / LDAP)
- **ajtarragona/alfresco-laravel** (client Alfresco) + `AlfrescoRestProvider` custom
- **elibyy/tcpdf-laravel**, **setasign/fpdi**, **setasign/fpdf** (generare/manipulare PDF — registre)
- **spatie/laravel-query-builder** (filtrare/sortare API)
- **guzzlehttp/guzzle** (HTTP către Alfresco / servicii externe)
- **fruitcake/laravel-cors**, **fideloper/proxy**

**Frontend**
- **Vue 2.6** + **Vuetify 2.4** (template Materio Admin), **Vue Router**, **Vuex**, **ApexCharts**, PDF viewer
- Build: **Laravel Mix / Webpack 5**, Babel; stil: ESLint, Prettier, StyleCI

**Infrastructură / date**
- **MySQL** (InnoDB), cache/sesiuni/queue configurabile (file/redis)
- **Alfresco ECM** (repository documente + aspecte), suport multi-instanță

**Testare / dev**
- PHPUnit 8.5, Mockery, Faker, `nunomaduro/collision`, `facade/ignition`

---

## 10. API-uri către aplicații externe (integrări consumate)

| Sistem extern | Adresă / API | Scop | Autentificare |
|---------------|--------------|------|----------------|
| **Alfresco UC** | `http://alf-uc.stsnet.ro:8080/alfresco/` (REST) | Repository principal documente + aspecte | user/parolă Alfresco (`.env`) |
| **Alfresco DMS** | `https://alf-dms.stsnet.ro/alfresco/` | Documente personale, CV DMRU, declarații, DPS | user/parolă Alfresco (`.env`) |
| **Active Directory (STS)** | `config/app.php` (hosts/base_dn), domeniu `sts.nest` | Autentificare utilizatori | bind LDAP cu cont de serviciu |
| **Redmine** | `<LinkIssueRedmine>.json` (DELETE) | Ștergere issue asociat unui document | header `X-Redmine-API-Key` |
| **Personal ERP** | `https://personal.sts.ro/api/erp/employees?filter[cnp]=...` | Date angajat după CNP (unitate, funcție) | cURL GET |
| **Aplicația „Personal”** (client → API-ul nostru) | `/alf-dms/upload-document`, `/upload-dps`, `/upload-declaration` | Trimite documente/DPS/declarații spre Alfresco | token „personal” cu expirare custom |
| **Kestra** (orchestrare, client → API-ul nostru) | `/alf-dms/import-cv` | Import CV DMRU (PDF + metadate CSV) în Alfresco | token „personal” cu expirare custom |
