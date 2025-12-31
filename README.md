# pfSense-Installation-Initial-Configuration-VMware-
מדריך מעשי שלב־אחר־שלב להתקנת pfSense בסביבת VMware Workstation, כולל הגדרת חומרה, רשת (WAN/LAN), התקנה מלאה של המערכת והגדרות ראשוניות. הפרויקט מתעד תהליך הקמה של Firewall ו־Router בסביבת מעבדה לצורכי לימוד סייבר, SOC, רשתות ואבטחת מידע, ומשמש בסיס לבניית מעבדות מתקדמות (Firewall Rules, Logs, IDS/IPS, VPN).
:

```md
## 🇮🇱 הסבר בעברית – שלב אחר שלב
```

---

## 🇮🇱 הסבר בעברית – מה עשינו בכל שלב ולמה

### 🔧 הגדרת מכונה וירטואלית (VMware)

בשלב זה יצרנו מכונה וירטואלית ייעודית ל־pfSense, שהוא **Firewall ו־Router** מבוסס FreeBSD.
המטרה היא לדמות סביבת רשת אמיתית לצורכי אבטחת מידע.

**למה VMware?**

* מאפשר עבודה בטוחה בלי לפגוע במערכת המארחת
* נפוץ מאוד במעבדות SOC
* מאפשר ריבוי כרטיסי רשת (קריטי ל־Firewall)

---

### 💾 הקצאת משאבים (RAM / CPU / Disk
<img width="539" height="551" alt="image" src="https://github.com/user-attachments/assets/692603d4-5ff6-4317-865e-e1101f10dcc5" />
<img width="449" height="424" alt="image" src="https://github.com/user-attachments/assets/43fbb38c-14c1-45b7-bba4-f9928bbe6496" />
<img width="434" height="422" alt="image" src="https://github.com/user-attachments/assets/6258763c-72a7-4aff-a6c7-ff1378bab71b" />


)

* **RAM ~3.3GB** – נדרש להפעלה יציבה של pfSense וה־WebGUI
* **CPU – 2 ליבות** – מאפשר ניתוח תעבורה ו־Firewall Rules
* **דיסק 20GB (Split)** – מספיק ללוגים ולתרגול, קל לגיבוי והעברה

🔍 זה מדמה Appliance אמיתי בארגון קטן–בינוני.

---

### 🌐 הגדרת כרטיסי רשת (שלב קריטי)


הגדרנו **שני כרטיסי רשת**, כי Firewall חייב להפריד בין רשתות:

| כרטיס     | תפקיד | הסבר                           |
| --------- | ----- | ------------------------------
|
| NAT       | WAN   | חיבור לאינטרנט דרך המחשב המארח |
| Host-Only | LAN   | רשת פנימית מאובטחת לניהול      |
<img width="754" height="698" alt="image" src="https://github.com/user-attachments/assets/46f772ea-efda-4b97-bbc6-a6749fe072b8" />
<img width="744" height="465" alt="image" src="https://github.com/user-attachments/assets/3c0b7bb6-6062-4138-a03f-4d3d7d37126f" />

<img width="441" height="393" alt="image" src="https://github.com/user-attachments/assets/b235d722-8369-462f-bd4d-59a0139d1d33" />


➡️ כך pfSense:

* מגן על ה־LAN
* שולט בתעבורה
* מאפשר ניהול דרך דפדפן

---

### 📀 אתחול מה־ISO

הפעלנו את ה־VM עם קובץ ההתקנה של pfSense.
בחרנו **Boot Multi User** – מצב אתחול רגיל למערכת
<img width="927" height="565" alt="image" src="https://github.com/user-attachments/assets/e0eb2bc3-45fd-437f-a79b-45b708f6f1eb" />
.

💡 אין צורך בשינויים מתקדמים בשלב זה.

---

### 📜 אישור רישיון
<img width="779" height="477" alt="image" src="https://github.com/user-attachments/assets/80594ed6-b64d-424c-98d9-772a60728c86" />


אישרנו את תנאי השימוש של pfSense.
זהו שלב פורמלי לפני התקנת מערכת הפעלה.

---

### ⚙️ בחירת 
<img width="803" height="490" alt="image" src="https://github.com/user-attachments/assets/3ac34819-db24-45c8-a2eb-c79d49de31fc" />
<img width="832" height="448" alt="image" src="https://github.com/user-attachments/assets/6649a475-56c2-4bfe-a8c1-d1664a1258ae" />

<img width="742" height="500" alt="image" src="https://github.com/user-attachments/assets/b45fbfe9-35a2-4120-bdb3-7a841621897e" />

Install pfSense

בחרנו באפשרות:

```
Install pfSense
```

➡️ התקנה חדשה של המערכת (לא שחזור ולא Rescue).

---

### ⌨️ בחירת מקלדת

בחרנו:

```
Continue with default keymap
```

🔹 המקלדת כאן רלוונטית רק לקונסול
🔹 הניהול האמיתי יתבצע דרך WebGUI

---

### 🗂️ חלוקת דיסק (Partitioning)

בחרנו:
<img width="802" height="462" alt="image" src="https://github.com/user-attachments/assets/adc42f6c-d77d-4f70-9769-8b5bf17941a5" />

```
Auto (UFS)
```

**למה UFS?**

* פשוט ויציב
* מתאים ל־VM
* פחות מורכב מ־ZFS
* אידיאלי ללמידה ו־PT

---

### ⏳ התקנה בפועל
<img width="763" height="475" alt="image" src="https://github.com/user-attachments/assets/9c23874e-9f76-4c7a-ae66-227d647639f6" />


בשלב זה:

* קבצי המערכת הועתקו לדיסק
* FreeBSD + pfSense הותקנו
* נוצר בסיס למערכת Firewall פעילה

---

### 🔁 סיום והפעלה מחדש

* דחינו כניסה ל־Shell ידני
* בחרנו **Reboot**
* הוצאנו את קובץ ה־ISO

➡️ חשוב כדי שהמערכת תעלה מהדיסק ולא מהאינסטולר.

---

### 🔌 אתחול ראשון ומיפוי ממשקים

pfSense זיהה אוטומטית את כרטיסי הרשת:

```text
WAN → em0 → DHCP
LAN → em1 → 192.168.1.1
```

✔️ דילגנו על VLANs
✔️ השתמשנו בהגדרה אוטומטית

זה מדמה Firewall ארגוני בסיסי.

---

### 🖥️ תפריט קונסול (Console Menu)

לאחר העלייה קיבלנו תפריט ניהול מקומי שמאפשר:

* ניהול רשת
* בדיקות תקשורת
* גישה ל־Shell
* הפעלת SSH
* צפייה בלוגים

📌 זהו כלי חשוב ל־SOC כאשר אין גישה ל־WebGUI.

---

### 🌐 גישה לממשק הניהול (WebGUI)

נכנסנו דרך הדפדפן לכתובת:

```
http://192.168.1.1
```

עם משתמש ברירת מחדל:

```
admin / pfsense
```

➡️ מכאן מתבצעת כל עבודת האבטחה:

* Firewall Rules
* NAT
* Logs
* IDS/IPS
* VPN

---

## 🧠 למה זה חשוב ל־PT / SOC?

המעבדה מדגימה:

* הבנה של הפרדת רשתות
* התקנת Firewall מאפס
* עבודה עם FreeBSD
* בסיס ל־Incident Detection
* הכנה ל־SIEM ול־Log Analysis

---

🔥
