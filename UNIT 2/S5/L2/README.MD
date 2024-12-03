# Esercizio S5/L2 - Scansioni con Nmap

Questo documento contiene il report delle scansioni effettuate su **Metasploitable** e un sistema Windows XP, utilizzando diverse tecniche offerte da Nmap.

## Obiettivi
- Identificare il sistema operativo (OS Fingerprinting).
- Effettuare una Syn Scan.
- Effettuare una TCP Connect Scan e confrontarne i risultati con la Syn Scan.
- Identificare le versioni dei servizi (Version Detection).
- Raccogliere informazioni su indirizzo IP, porte aperte e servizi in ascolto con versione.

---

## 1. Target: Metasploitable
### **Informazioni Generali**
- **Indirizzo IP:** `192.168.50.101`
- **Sistema Operativo Identificato:** Linux 2.6.x  
  - **Dettagli:** Kernel Linux 2.6.9 - 2.6.33  
  - **Dispositivo di Tipo:** General Purpose (VirtualBox)

---

### **1.1. OS Fingerprint (-O)**
- **Tecnica:** Rilevamento del sistema operativo.
- **Risultato:** Linux 2.6.x, Kernel 2.6.9 - 2.6.33.

---

### **1.2. Syn Scan (-sS)**
- **Tecnica:** Scansione SYN, invia pacchetti SYN e attende una risposta senza completare la connessione.
- **Risultati Porte Aperte:**
  - **21/tcp** (FTP)
  - **22/tcp** (SSH)
  - **23/tcp** (Telnet)
  - **25/tcp** (SMTP)
  - **53/tcp** (DNS)
  - **80/tcp** (HTTP)
  - **3306/tcp** (MySQL)
  - ... (consultare il file per l'elenco completo)

---

### **1.3. TCP Connect Scan (-sT)**
- **Tecnica:** Scansione TCP completa con 3-way handshake.
- **Risultati:**  
  - Porte aperte rilevate identiche alla Syn Scan.  
  - Differenza principale: TCP Connect stabilisce connessioni complete, risultando più evidente nei log del sistema bersaglio.

---

### **1.4. Version Detection (-sV)**
- **Tecnica:** Identificazione delle versioni dei servizi attivi sulle porte aperte.
- **Risultati Identificati:**
  - **21/tcp (FTP):** vsftpd 2.3.4
  - **22/tcp (SSH):** OpenSSH 4.7p1 Debian
  - **25/tcp (SMTP):** Postfix smtpd
  - **53/tcp (DNS):** ISC BIND 9.4.2
  - **80/tcp (HTTP):** Apache 2.2.8 ((Ubuntu) DAV/2)
  - **3306/tcp (MySQL):** MySQL 5.0.51a
  - **5432/tcp (PostgreSQL):** PostgreSQL 8.3.0 - 8.3.7
  - Altri dettagli disponibili nel report.

---

## 2. Target: Windows XP
### **Informazioni Generali**
- **Indirizzo IP:** `192.168.50.102`
- **Sistema Operativo Identificato:** Windows XP o Server 2003
  - **Versioni Possibili:**
    - Microsoft Windows XP SP0/SP2/SP3
    - Microsoft Windows Server 2003 (varie edizioni).

---

### **2.1. OS Fingerprint (-O)**
- **Tecnica:** Identificazione del sistema operativo tramite fingerprinting.
- **Risultato:**  
  - Microsoft Windows XP SP0/SP2/SP3 o Server 2003.  
  - Affidabilità: Risultati basati su porte limitate disponibili (139/tcp e 445/tcp).

---

## 3. Confronto Syn Scan e TCP Connect
| **Parametro**         | **Syn Scan (-sS)**          | **TCP Connect (-sT)**       |
|-----------------------|-----------------------------|-----------------------------|
| **Velocità**          | Più veloce                 | Più lento                  |
| **Discrezione**       | Meno rilevabile            | Più evidente nei log       |
| **Connessioni**       | Non completa la connessione | Completa la connessione    |
| **Risultati Porte**   | Identici                   | Identici                   |

---

## Conclusioni
1. **Sistema Operativo Metasploitable:** Linux (Kernel 2.6.x).
2. **Sistema Operativo Windows XP:** Windows XP SP0/SP2/SP3 o Server 2003.
3. **Porte Aperte su Metasploitable:** Numerose porte critiche aperte, tra cui FTP, HTTP, MySQL, e root shell.
4. **Versioni Servizi su Metasploitable:** Identificazione precisa di numerosi servizi, incluse versioni di Apache, MySQL, PostgreSQL e altri.
5. **Differenze Syn Scan e TCP Connect:**  
   - Syn Scan è più discreto e adatto a scansioni stealth.
   - TCP Connect è utile in ambienti con restrizioni, ma più facilmente rilevabile.

---

## Raccomandazioni
- **Metasploitable:** Rafforzare i servizi esposti e rimuovere servizi non necessari per ridurre i rischi di sfruttamento.
- **Windows XP:** Il sistema operativo è obsoleto e deve essere aggiornato o sostituito per mitigare vulnerabilità note.
