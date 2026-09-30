# Flux Alfresco – Redmine

> 📚 Procedurile complete se regăsesc în **Redmine → Sistem de Management de documente → Wiki**.

Documentul descrie pașii de lucru pentru cele **5 tipuri de flux** gestionate în Alfresco și Redmine:

| Flux | Fișier încărcat | Template |
| --- | --- | --- |
| 🟦 Cereri de servicii | `.pdf` | 1. Doc. intrare |
| 🟩 Petiții 112 | `.pdf` | 10. Petiție 112 |
| 🟧 Document ieșire STS | `.docx` | 8. Doc ieșire |
| 🟪 Document intrare / Document intern | `.pdf` | 1. Doc. intrare |
| 🟥 Document electronic | `.pdf` *(generat, nu scanat)* | File-registration |

---

## Etapa I – Alfresco: încărcare și indexare

### 1. Încărcare document (upload)

Comun pentru toate fluxurile. Exemplu de denumire: **`123456 25-03-2025`**

### 2. Indexare document (*Edit properties*)

Se completează câmpurile **Name** și **Description**.

> ⚠️ Datele introduse **nu trebuie să se repete** de la un document la altul.

### 3. Starea / tipul documentului

| Flux | Câmp de completat |
| --- | --- |
| Cereri de servicii | Starea document: **Document intrare STS** |
| Petiții 112 | Starea document: **Petiție 112** |
| Document ieșire STS | Starea document: **Document ieșire STS** |
| Document intrare / intern | Starea document: **Document intrare / Document intern** |
| Document electronic | Tip document: **Document electronic** |

> 💡 **Observație:** În funcție de **folderul** în care se încarcă documentul și de câmpul **Stare document**, rulează un anumit script pentru un anumit flux.

### 4. Salvare și refresh

### 5. Mutarea automată a documentului

| Flux | Ce se întâmplă |
| --- | --- |
| Cereri de servicii | Documentul se mută automat în site-ul **Transfer STS**, folderul `Documente partajate` |
| Petiții 112 | Documentul se mută automat în site-ul **Transfer STS**, folderul `Petitii 112` |
| Document ieșire STS | Se apasă butonul **Document partajate** → documentul se mută automat în site-ul **Transfer STS**, folderul `Documente partajate` |
| Document intrare / intern | Documentul se mută automat în **site-ul unității**, folderul `Document in lucru` |
| Document electronic | Documentul se încarcă automat în Alfresco – site-ul **Documente electronice**, folderul unității |

---

## Etapa II – Redmine: issue în spațiul unității

### 6. Creare issue

Se creează un issue în Redmine, în **spațiul propriu al unității** (folderul unității).

| Flux | Spațiu Redmine | Tip issue |
| --- | --- | --- |
| Cereri de servicii | Intrare STS – DTI | Cerere de servicii |
| Petiții 112 | Intrare STS – DTI | Petiție 112 |
| Document ieșire STS | Ieșire STS – DTI | Document de ieșire |
| Document intrare / intern | DMS-STS – ATH – DTI | Document intern |
| Document electronic | Intrare STS – DTI | Document electronic |

### 7. Relaționare, subtask-uri și închidere

| Flux | Relaționare | Subtask-uri | Închidere |
| --- | --- | --- | --- |
| Cereri de servicii | – | – | La finalizare: **100%**, status **Close** |
| Petiții 112 | – | – | La finalizare: **100%**, status **Close** |
| Document ieșire STS | Cu documentul de intrare | ✅ Da | Status **Close** la finalizarea lucrării |
| Document intrare / intern | Cu alte documente de interes pentru lucrare | ✅ Da | Status **Close** la finalizarea lucrării |
| Document electronic | Cu alte documente de interes sau legate de lucrare | ✅ Da | Status **Close** când **toate unitățile** destinatare au subtask-ul în **Close** |

---

## Etapa III – Partajare în Alfresco

### 8. Acțiune după finalizare / partajare

| Flux | Acțiune |
| --- | --- |
| Cereri de servicii | **Copy to → CREATE LINK** |
| Petiții 112 | **Copy to → CREATE LINK** |
| Document ieșire STS | **Copy to → CREATE LINK** |
| Document intrare / intern | După finalizarea lucrării, documentul se mută cu **Move to** din `Documente in lucru` în `Documente rezolvate` |
| Document electronic | 1. Documentul se **semnează electronic** de șeful unității<br>2. **Copy to → CREATE LINK** |

### 9. Destinația link-ului

| Flux | Site | Path |
| --- | --- | --- |
| Cereri de servicii | Transfer STS | Intrare STS |
| Petiții 112 | Transfer STS | Petitie 112 |
| Document ieșire STS | Transfer STS | STS – PA – DCRB |
| ↳ *dacă se lucrează colaborativ* | Transfer STS | STS – unitatea X *(link și către fiecare unitate implicată)* |
| Document intrare / intern | – | – |
| Document electronic | Documente electronice | Folderul unității de interes **sau** Unități centrale **sau** Unități teritoriale |

> 💡 **Observație:** Copierea link-ului este necesară pentru **acordarea permisiunilor** personalului din unitatea militară care trebuie să lucreze colaborativ pe document.
> *(Se aplică fluxurilor: Cereri de servicii, Petiții 112, Document ieșire STS.)*

> ⚠️ **Atenție:** Dacă s-a apăsat butonul **Create link** pe un document **indexat greșit**, link-ul creat „rămâne agățat”, iar ulterior **scriptul nu mai rulează**, chiar dacă indexarea este corectată.
> *(Se aplică fluxurilor: Cereri de servicii, Petiții 112, Document ieșire STS.)*

---

## Etapa IV – Procesare ulterioară

### 🟦 Cereri de servicii

1. Se creează link la **DJSG** pentru verificarea documentului. Dacă este cerere de servicii, **DJTS** trimite documentul la **MD**.
2. Se creează link de MD în **DMS-STS – Cereri de servicii**.
3. Se creează **Repartiții de MD** și se relaționează cu cererea de servicii de la nivelul unității.
4. Fiecare unitate creează **rezoluții** către personalul din subordine, cu butonul **Adaugă rezoluție**.

### 🟩 Petiții 112

1. Se creează **automat** issue în Redmine în **DMS-STS – Petiție 112**.
2. Se creează **subtask-uri** către unitățile de interes.

### 🟧 Document ieșire STS

1. Se creează **automat** issue în Redmine în **Transfer intern STS – Unitate (DCRB) – Transfer**.
2. **DCRB** face solicitările de aprobare.
3. După obținerea tuturor aprobărilor și întocmirea documentului în formă finală, acesta se **versionează** cu formatul `.pdf` aprobat.

### 🟪 Document intrare / Document intern

Nu există pași suplimentari în această etapă.

### 🟥 Document electronic

1. Se creează issue în Redmine pentru **unitățile către care a fost transmis** documentul.
2. Fiecare unitate poate crea **subtask**.

---

## ✅ Regulă generală de închidere

- Fiecare issue trebuie trecut la **100%** și în status **Close** la finalizarea lucrării.
- Un **issue părinte** poate fi închis **doar după ce toate subtask-urile** sunt în status **Close**.
