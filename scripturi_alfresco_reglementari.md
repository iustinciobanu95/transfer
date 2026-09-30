# Situația centralizată a scripturilor din Alfresco – Reglementări

## Cuprins

1. [Declarații de avere și interese](#1-declarații-de-avere-și-interese)
2. [FST2018](#2-fst2018)
3. [AVIZE](#3-avize)
4. [Reglementări](#4-reglementări)
5. [Proiecte de instalare Echipamente de Telecomunicații – modernizare TETRA](#5-proiecte-de-instalare-echipamente-de-telecomunicații--modernizare-tetra)
6. [Observații / neconcordanțe de verificat](#6-observații--neconcordanțe-de-verificat)

### Privire de ansamblu

| Site | Directoare | Nr. scripturi | Tip automatizare |
| --- | --- | --- | --- |
| Declarații de avere și interese | `documentLibrary` | 10 | Permisiuni, metadate, mailuri, notificări portal |
| FST2018 | `5. NR implementare FST`, `2 Fisa in curs de avizare` (pe județe) | 42 + 10 | Issue Redmine, fluxuri de avizare |
| AVIZE | `2 AVIZE` (pe județe) | 6 | Fluxuri de avizare |
| Reglementări | `DTI - DTI2018*`, `Reglementari` | 30 | Permisiuni, metadate, issue Redmine |
| Proiecte TETRA | `2.Proiecte in curs de avizare`, `3.Proiecte avizate` | 4 | Fluxuri de avizare, issue Redmine |

---

## 1. Declarații de avere și interese

| | |
| --- | --- |
| **Director rulare script** | `documentLibrary` |
| **Link folder** | `Scripturi Declaratii Avere Interese` |

### Regulile directorului (în ordinea de rulare)

| # | Regulă | Detalii |
| --- | --- | --- |
| 1 | Add Categorie | |
| 2 | Add Aspect | |
| 3 | Setare AN | |
| 4 | Add Id | |
| 5 | Setare Permisiuni | |
| 6 | Setare Permisiuni Consilier 1 | |
| 7 | Update Metade | |
| 8 | Trimitere mail | |
| 9 | Notificare adaugare | Notificare către portal la încărcarea unei declarații de avere |
| 10 | StareDocumentDeclaratieAvere | |
| 11 | StareDocumentDeclaratieInterese | |
| 12 | Trimitere mail invalidare | |
| 13 | Trimitere mail validare declaratie de interese | |
| 14 | Trimitere mail validare declaratie de interese Consilier 1 | |
| 15 | Trimitere mail validare declaratie de avere | |
| 16 | Trimitere email declaratie avere Consilier 1 | |
| 17 | Notificare stergere declaratie de avere | Notificare către portal pentru ștergerea unei declarații de avere |
| 18 | Notificare modificare/validare | Notificare către portal pt. modificarea/validarea unei declarații de avere |

### Scripturi

| Script | Descriere | Trigger |
| --- | --- | --- |
| `0SetPermisiuniDeclaratiiAvereInterese.js` | Ridică permisiunile moștenite și dă permisiuni creatorului și grupului `GROUP_DMRU2018S` | La upload document |
| `0UpdateMetadateDeclaratAvere.js` | Completează automat câmpurile | La upload document |
| `0ScriptMailDepunere.js` | Trimite mail creatorului la depunere | La upload document |
| `0CounterDeclaratie.js` | Adaugă un număr automat pe document | La upload document |
| `0SetareTipDeclaratie.js` | Setare tip declarație de avere / interese | Setarea tipului de declarație în metadate de către DMRU |
| `0ScriptMailInvalidare.js` | Trimite mail când se invalidează declarația de avere | La ștergerea documentului |
| `0ScriptMailValidare.js` | Trimite mail când se validează declarația de avere și declarația de interese | La validarea de către DMRU |
| `declaration_created.js` | Trimite notificare când se creează declarația de avere | La validarea de către DMRU |
| `declaration_deleted.js` | Trimite notificare când se invalidează declarația de avere | La ștergerea documentului |
| `declaration_modified.js` | Trimite notificare când se validează declarația de avere | La setarea tipului de declarație |

---

## 2. FST2018

### 2.1. Creare issue Redmine – `5. NR implementare FST`

Toate scripturile din această categorie au același comportament:

| | |
| --- | --- |
| **Link folder** | `Repository - Data Dictionary - Scripts - Issue Redmine - Creare Issue Redmine CS Raspuns Catre Beneficiari` |
| **Descriere** | Se creează link în Redmine, în proiectul unității, pe tracker **Document intern** |
| **Trigger** | La update-ul metadatelor |
| **Director** | `Documents - <Județ> - 5. NR implementare FST` |
| **Șablon script** | `Creare_Issue_Redmine-CS_PA_DJTS_<COD>-Raspuns_catre_beneficiari.js` |

| Județ / unitate | Cod | Script |
| --- | --- | --- |
| Alba | AB | `Creare_Issue_Redmine-CS_PA_DJTS_AB-Raspuns_catre_beneficiari.js` |
| Arad | AR | `Creare_Issue_Redmine-CS_PA_DJTS_AR-Raspuns_catre_beneficiari.js` |
| Argeș | AG | `Creare_Issue_Redmine-CS_PA_DJTS_AG-Raspuns_catre_beneficiari.js` |
| Bacău | BC | `Creare_Issue_Redmine-CS_PA_DJTS_BC-Raspuns_catre_beneficiari.js` |
| Bihor | BH | `Creare_Issue_Redmine-CS_PA_DJTS_BH-Raspuns_catre_beneficiari.js` |
| Bistrița-Năsăud | BN | `Creare_Issue_Redmine-CS_PA_DJTS_BN-Raspuns_catre_beneficiari.js` |
| Botoșani | BT | `Creare_Issue_Redmine-CS_PA_DJTS_BT-Raspuns_catre_beneficiari.js` |
| Brăila | BR | `Creare_Issue_Redmine-CS_PA_DJTS_BR-Raspuns_catre_beneficiari.js` |
| Brașov | BV | `Creare_Issue_Redmine-CS_PA_DJTS_BV-Raspuns_catre_beneficiari.js` |
| **București-Ilfov** ⚠️ | DCPD | `Creare_Issue_Redmine-CS_DCPD-Raspuns_catre_beneficiari.js` |
| Buzău | BZ | `Creare_Issue_Redmine-CS_PA_DJTS_BZ-Raspuns_catre_beneficiari.js` |
| Călărași | CL | `Creare_Issue_Redmine-CS_PA_DJTS_CL-Raspuns_catre_beneficiari.js` |
| Caraș-Severin | CS | `Creare_Issue_Redmine-CS_PA_DJTS_CS-Raspuns_catre_beneficiari.js` |
| Cluj | CJ | `Creare_Issue_Redmine-CS_PA_DJTS_CJ-Raspuns_catre_beneficiari.js` |
| Constanța | CT | `Creare_Issue_Redmine-CS_PA_DJTS_CT-Raspuns_catre_beneficiari.js` |
| Covasna | CV | `Creare_Issue_Redmine-CS_PA_DJTS_CV-Raspuns_catre_beneficiari.js` |
| Dâmbovița | DB | `Creare_Issue_Redmine-CS_PA_DJTS_DB-Raspuns_catre_beneficiari.js` |
| Dolj | DJ | `Creare_Issue_Redmine-CS_PA_DJTS_DJ-Raspuns_catre_beneficiari.js` |
| Galați | GL | `Creare_Issue_Redmine-CS_PA_DJTS_GL-Raspuns_catre_beneficiari.js` |
| Giurgiu | GR | `Creare_Issue_Redmine-CS_PA_DJTS_GR-Raspuns_catre_beneficiari.js` |
| Gorj | GJ | `Creare_Issue_Redmine-CS_PA_DJTS_GJ-Raspuns_catre_beneficiari.js` |
| Harghita | HR | `Creare_Issue_Redmine-CS_PA_DJTS_HR-Raspuns_catre_beneficiari.js` |
| Hunedoara | HD | `Creare_Issue_Redmine-CS_PA_DJTS_HD-Raspuns_catre_beneficiari.js` |
| Ialomița | IL | `Creare_Issue_Redmine-CS_PA_DJTS_IL-Raspuns_catre_beneficiari.js` |
| Iași | IS | `Creare_Issue_Redmine-CS_PA_DJTS_IS-Raspuns_catre_beneficiari.js` |
| Maramureș | MM | `Creare_Issue_Redmine-CS_PA_DJTS_MM-Raspuns_catre_beneficiari.js` |
| Mehedinți | MH | `Creare_Issue_Redmine-CS_PA_DJTS_MH-Raspuns_catre_beneficiari.js` |
| Mureș | MS | `Creare_Issue_Redmine-CS_PA_DJTS_MS-Raspuns_catre_beneficiari.js` |
| Neamț | NT | `Creare_Issue_Redmine-CS_PA_DJTS_NT-Raspuns_catre_beneficiari.js` |
| Olt | OT | `Creare_Issue_Redmine-CS_PA_DJTS_OT-Raspuns_catre_beneficiari.js` |
| Prahova | PH | `Creare_Issue_Redmine-CS_PA_DJTS_PH-Raspuns_catre_beneficiari.js` |
| Sălaj | SJ | `Creare_Issue_Redmine-CS_PA_DJTS_SJ-Raspuns_catre_beneficiari.js` |
| Satu Mare | SM | `Creare_Issue_Redmine-CS_PA_DJTS_SM-Raspuns_catre_beneficiari.js` |
| Sibiu | SB | `Creare_Issue_Redmine-CS_PA_DJTS_SB-Raspuns_catre_beneficiari.js` |
| Suceava | SV | `Creare_Issue_Redmine-CS_PA_DJTS_SV-Raspuns_catre_beneficiari.js` |
| Teleorman | TR | `Creare_Issue_Redmine-CS_PA_DJTS_TR-Raspuns_catre_beneficiari.js` |
| Timiș | TM | `Creare_Issue_Redmine-CS_PA_DJTS_TM-Raspuns_catre_beneficiari.js` |
| Tulcea | TL | `Creare_Issue_Redmine-CS_PA_DJTS_TL-Raspuns_catre_beneficiari.js` |
| Vâlcea | VL | `Creare_Issue_Redmine-CS_PA_DJTS_VL-Raspuns_catre_beneficiari.js` |
| Vaslui | VS | `Creare_Issue_Redmine-CS_PA_DJTS_VS-Raspuns_catre_beneficiari.js` |
| Vrancea | VN | `Creare_Issue_Redmine-CS_PA_DJTS_VN-Raspuns_catre_beneficiari.js` |
| **CPP_BV** ⚠️ | CPP | `Creare_Issue_Redmine-CS_CPP-Raspuns_catre_beneficiari.js` |

> ⚠️ **București-Ilfov** și **CPP_BV** nu respectă șablonul `CS_PA_DJTS_<COD>`.

### 2.2. Fluxuri de avizare – `2 Fisa in curs de avizare`

| | |
| --- | --- |
| **Director (exemplu)** | `Documents - Alba - 2 Fisa in curs de avizare` |
| **Link folder** | `Repository - Data Dictionary - Scripts - FST_PRODUCTIE` |

> 📁 Acest folder se regăsește în **toate directoarele de județe**, cu aceleași scripturi.

Toate scripturile trimit mail și lansează flux către grupul `FST2018<STRUCTURĂ>`, dacă documentul are setată categoria respectivă.

| Script | Flux lansat către | Trigger (categorie setată) |
| --- | --- | --- |
| `start-pooled-review-workflowFST_AVIZ_DCAOSR.js` | `FST2018DCAOSR` | DCAOSR |
| `start-pooled-review-workflowFST_AVIZ_DCDSSI.js` | `FST2018DCDSSI` | DCDSSI |
| `start-pooled-review-workflowFST_AVIZ_DCPD.js` | `FST2018DCPD` | DCPD |
| `start-pooled-review-workflowFST_AVIZ_DCRB.js` | `FST2018DCRB` | DCRB |
| `start-pooled-review-workflowFST_AVIZ_DESA.js` | `FST2018DESA` | DESA |
| `start-pooled-review-workflowFST_AVIZ_DESR.js` | `FST2018DESR` | DESR |
| `start-pooled-review-workflowFST_AVIZ_DL.js` | `FST2018DL` | DL |
| `start-pooled-review-workflowFST_AVIZ_DOES.js` | `FST2018DOES` | DOES |
| `start-pooled-review-workflowFST_AVIZ_DSCSTI.js` | `FST2018DSCSTI` | DSCSTI |
| `start-pooled-review-workflowFST_AVIZ_DTI.js` | `FST2018DTI` | DTI |

---

## 3. AVIZE

> 📁 Folderele de mai jos se regăsesc în **toate directoarele de județe**, cu scripturile aferente. Toate scripturile trimit mail și lansează flux dacă sunt setate categoriile indicate.

### 3.1. Aviz MLPDA

| | |
| --- | --- |
| **Director (exemplu)** | `Documents - Avize MDLPA - Alba - 2 AVIZE` |
| **Link folder** | `Repository - Data Dictionary - Scripts - Avize_STS - Aviz MLPDA` |

| Script | Flux lansat către | Trigger (categorii setate) |
| --- | --- | --- |
| `AVIZ_AB_MLPDA_Start-pooled-review-workflow.js` | `AB_MDLAP` | Județ **și** MDLAP |
| `AVIZ_DL_MLPDA_Start-pooled-review-workflow.js` | `DL_MDRAP` | DL **și** MDLAP |

### 3.2. Aviz ANRSPS

| | |
| --- | --- |
| **Director (exemplu)** | `Documents - Avize MDLPA - Alba - 2 AVIZE` *(așa apare în document)* |
| **Link folder** | `Repository - Data Dictionary - Scripts - Avize_STS - Aviz ANRSPS` |

| Script | Flux lansat către | Trigger (categorii setate) |
| --- | --- | --- |
| `AVIZ_AB_ANRSPS_Start-pooled-review-workflow.js` | `AB_ANRSPS` | AB **și** ANRSPS |
| `AVIZ_DL_ANRSPS_Start-pooled-review-workflow.js` | `DL_ANRSPS` | DL **și** ANRSPS |

### 3.3. Aviz Legea 17

| | |
| --- | --- |
| **Director (exemplu)** | `Documents - Avize Legea 17 - Alba - 2 AVIZE` |
| **Link folder** | `Repository - Data Dictionary - Scripts - Avize_STS - Aviz Legea 17` |

| Script | Flux lansat către | Trigger (categorii setate) |
| --- | --- | --- |
| `AVIZ_AB_SMEC_Start-pooled-review-workflow.js` | `AB_SMEC` | AB **și** Legea 17 |
| `AVIZ_DL_SMEC_Start-pooled-review-workflow.js` | `DL_SMEC` | DL **și** Legea 17 |

---

## 4. Reglementări

### 4.1. Setare permisiuni pe grupuri – `Documents - DTI - DTI2018*`

> 📁 Acest folder se regăsește în **toate directoarele de unități**, cu scripturile aferente. În loc de `DTI2018` se completează **grupul aferent structurii din unitate**.

| | |
| --- | --- |
| **Link folder** | `Repository - Data Dictionary - Scripts - Setare Permisiuni Reglementari - DTI` |
| **Descriere** | Se acordă permisiune pe documentul de bază la care se creează link-ul în director |
| **Trigger** | Se setează permisiunea pentru grup pe document |

**Script suplimentar în `Documents - DTI - DTI2018`:**

| Script | Link folder | Descriere |
| --- | --- | --- |
| `ZZZUpdate_Metadate_Link_Document.js` | `Repository - Data Dictionary - Scripts` | Face update la metadate pe link-ul documentului |

**Directoare și scripturi de permisiuni:**

| Grup / structură | Director | Script |
| --- | --- | --- |
| DTI2018 | `Documents - DTI - DTI2018` | `SetPermisiuniDTI2018.js` |
| Administrativ | `Documents - DTI - DTI2018Administrativ` | `SetPermisiuniDTI2018Administrativ.js` |
| BIAS | `Documents - DTI - DTI2018BIAS` | `SetPermisiuniDTI2018BIAS.js` |
| Conducere ⚠️ | `Documents - DTI - DTI2018Conducere` | `SetPermisiuniDTI2018Loctiitori.js` |
| S1 | `Documents - DTI - DTI2018S1` | `SetPermisiuniDTI2018S1.js` |
| S1B1 | `Documents - DTI - DTI2018S1B1` | `SetPermisiuniDTI2018S1B1.js` |
| S1B2 | `Documents - DTI - DTI2018S1B2` | `SetPermisiuniDTI2018S1B2.js` |
| S1B3 | `Documents - DTI - DTI2018S1B3` | `SetPermisiuniDTI2018S1B3.js` |
| S1B4 | `Documents - DTI - DTI2018S1B4` | `SetPermisiuniDTI2018S1B4.js` |
| S2 | `Documents - DTI - DTI2018S2` | `SetPermisiuniDTI2018S2.js` |
| S2B1 | `Documents - DTI - DTI2018S2B1` | `SetPermisiuniDTI2018S2B1.js` |
| S2B2 | `Documents - DTI - DTI2018S2B2` | `SetPermisiuniDTI2018S2B2.js` |
| S2B3 | `Documents - DTI - DTI2018S2B3` | `SetPermisiuniDTI2018S2B3.js` |
| S3 | `Documents - DTI - DTI2018S3` | `SetPermisiuniDTI2018S3.js` |
| S3B1 | `Documents - DTI - DTI2018S3B1` | `SetPermisiuniDTI2018S3B1.js` |
| S3B2 | `Documents - DTI - DTI2018S3B2` | `SetPermisiuniDTI2018S3B2.js` |
| S4 | `Documents - DTI - DTI2018S4` | `SetPermisiuniDTI2018S4.js` |
| S4B1 | `Documents - DTI - DTI2018S4B1` | `SetPermisiuniDTI2018S4B1.js` |
| S4B2 | `Documents - DTI - DTI2018S4B2` | `SetPermisiuniDTI2018S4B2.js` |
| S4B3 | `Documents - DTI - DTI2018S4B3` | `SetPermisiuniDTI2018S4B3.js` |
| Secretariat | `Documents - DTI - DTI2018Secretariat` | `SetPermisiuniDTI2018Secretariat.js` |
| Șef BIAS | `Documents - DTI - DTI2018SefBIAS` | `SetPermisiuniDTI2018SefBIAS.js` |
| Șefi birouri | `Documents - DTI - DTI2018SefiBirouri` | `SetPermisiuniDTI2018SefiBirouri.js` |
| Șefi sectoare | `Documents - DTI - DTI2018SefiSectoare` | `SetPermisiuniDTI2018SefiSectoare.js` |
| Șefi sectoare și birouri | `Documents - DTI - DTI2018SefiSectoareBirouri` | `SetPermisiuniDTI2018SefiSectoareBirouri.js` |
| Șef S1 | `Documents - DTI - DTI2018SefS1` | `SetPermisiuniDTI2018SefS1.js` |
| Șef S2 | `Documents - DTI - DTI2018SefS2` | `SetPermisiuniDTI2018SefS2.js` |
| Șef S3 | `Documents - DTI - DTI2018SefS3` | `SetPermisiuniDTI2018SefS3.js` |
| Șef S4 | `Documents - DTI - DTI2018SefS4` | `SetPermisiuniDTI2018SefS4.js` |

> ⚠️ Directorul **Conducere** folosește scriptul `...Loctiitori.js`, nu `...Conducere.js`.

### 4.2. Creare issue Redmine – `Documents - Reglementari`

| Script | Link folder | Descriere |
| --- | --- | --- |
| `Creare_Issue_Redmine--Reglementari.js` | `Repository - Data Dictionary - Scripts - Issue redmine - Creare Issue Redmine Reglementari` | Se creează link în Redmine, în proiectul **Reglementări**, pe tracker **Reglementări** |

---

## 5. Proiecte de instalare Echipamente de Telecomunicații – modernizare TETRA

### 5.1. Fluxuri de avizare – `2.Proiecte in curs de avizare`

| | |
| --- | --- |
| **Director (exemplu)** | `Documents - ALBA - 2.Proiecte in curs de avizare` |
| **Link folder** | `Repository - Data Dictionary - Scripts - Fluxuri_Proiecte` |

| Script | Descriere | Trigger |
| --- | --- | --- |
| `1start-pooled-review-workflowProiecte_DCPD.js` | Lansează flux la **DCPD** | Trimite flux la DESA când categoria este *Proiect avizat DCPD* |
| `1start-pooled-review-workflowProiecte_DESA.js` | Lansează flux la **DESA** | Trimite flux la DESA când categoria este *Proiect avizat DESA* |
| `1start-pooled-review-workflowProiecte_DESR.js` | Lansează flux la **DESR** | Trimite flux la DESA când categoria este *Proiect avizat DESR* |

### 5.2. Creare issue Redmine – `3.Proiecte avizate`

| | |
| --- | --- |
| **Director (exemplu)** | `Documents - ALBA - 3.Proiecte avizate` |
| **Script** | `Creare_Issue_Redmine-DJTS_AB-Proiecte_instalare_echipamente.js` |
| **Link folder** | `Repository - Data Dictionary - Scripts - Issue redmine - Creare Issue Redmine Proiecte Instalare Echipamente` |
| **Descriere** | Se creează link în Redmine, în proiectul **DCPD**, pe tracker **Proiecte instalare** |
| **Trigger** | Se creează issue în Redmine când documentul este mutat în acest folder |

---

## 6. Observații / neconcordanțe de verificat

Câteva locuri din documentul original în care textul pare inconsecvent (păstrat ca atare mai sus):

- **`declaration_created.js`**: descrierea spune „la crearea declarației”, iar trigger-ul „la validarea de către DMRU” (regula asociată este *Notificare adaugare*, la încărcare).
- **`declaration_deleted.js`** și **`0ScriptMailInvalidare.js`**: descrierea spune „la invalidare”, trigger-ul „la ștergerea documentului”.
- **`declaration_modified.js`**: descrierea spune „la validare”, trigger-ul „la setarea tipului de declarație”.
- **AVIZE – ANRSPS**: folderul apare cu același nume ca la MLPDA (`Avize MDLPA`); probabil ar trebui să fie `Avize ANRSPS`.
- **AVIZE – MLPDA**: denumirile alternează între *MLPDA*, *MDLAP*, *MDLPA* și *MDRAP*.
- **Proiecte TETRA**: toate cele trei scripturi (DCPD, DESA, DESR) spun „trimite flux la DESA” în trigger, probabil copiere repetată din rândul DESA.

---

📎 **Anexă:** `MAE_SMD_GL-Site…ation-v1.0.xlsx` *(numele fișierului este trunchiat în documentul original)*
