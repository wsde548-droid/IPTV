# IPTV נגן — הוראות בנייה, התקנה והרצה (עברית)

פרויקט אנדרואיד שעוטף את הנגן שלך (index.html) בתוך WebView, ורץ גם על טלפון/טאבלט וגם על Android TV.

## למה APK ולא דפדפן?
בדפדפן רגיל שתי חסימות מונעות חיבור לשרתי IPTV: **CORS** ו-**Mixed-Content** (דף https מול שרת http).
בתוך WebView באפליקציה אין את החסימות האלו — לכן החיבור לשרת http://line.trex4k.top אמור לעבוד ישירות.

---

## שלב 1 — בניית ה-APK

### אפשרות A — Android Studio (מומלץ, חינמי)
1. התקן **Android Studio** (ל-Windows / Mac / Linux, חינם).
2. פתח אותו → **Open** → בחר את תיקיית הפרויקט הזו (התיקייה שמכילה settings.gradle).
3. המתן ל-**Gradle Sync** (אוטומטי; יוריד את הרכיבים החסרים).
4. תפריט **Build → Build Bundle(s) / APK(s) → Build APK(s)**.
5. בסיום יופיע קישור **locate** — ה-APK יהיה ב:
   `app/build/outputs/apk/debug/app-debug.apk`

### אפשרות B — שורת פקודה (אם מותקן Android SDK)
```bash
cd apk
./gradlew assembleDebug        # Windows: gradlew.bat assembleDebug
# ה-APK: app/build/outputs/apk/debug/app-debug.apk
```

### אפשרות C — בלי להתקין כלום (אתר המרה)
יש אתרים שהופכים HTML ל-APK. העלה את הקובץ `iptv-player.html`.
ודא שמסומנות האפשרויות: **Cleartext / HTTP** ו-**DOM Storage / localStorage**.

---

## שלב 2 — התקנה על טלפון / טאבלט
1. העבר את `app-debug.apk` לטלפון (כבל USB / דראייב / וואטסאפ לעצמך).
2. בטלפון → הגדרות → אפשר “התקנה ממקורות לא ידועים”.
3. פתח את קובץ ה-APK והתקן.

---

## שלב 3 — התקנה על הטלוויזיה

> חשוב: ניתן להתקין **רק על טלוויזיות מבוססות אנדרואיד**:
> - ✅ **Android TV / Google TV** (Sony, Philips, TCL, Xiaomi, ועוד), וכן **Amazon Fire TV**.
> - ❌ **Samsung (Tizen)** ו-**LG (webOS)** לא תומכות בקבצי APK.
>   בהן הפתרון הוא להשתמש בממיר חיצוני (למשל Fire TV Stick) או בדפדפן הטלוויזיה.

הפרויקט כבר מוגדר ל-Android TV (מופיע במסך הבית, מצב רוחבי, ללא דרישת מסך מגע).

### דרך 1 — אפליקציית “Downloader” (הכי פשוט)
1. בחנות האפליקציות של הטלוויזיה התקן את האפליקציה **Downloader**.
2. העלה את קובץ ה-APK למקום נגיש (למשל Google Drive / אחסון אישי) וקבל קישור ישיר להורדה.
3. הקלד את הקישור ב-Downloader → הורד → התקן.
4. אפשר “התקנה ממקורות לא ידועים” כשמתבקשים.

### דרך 2 — USB
1. העתק את קובץ ה-APK לדיסק-ונ-קי.
2. חבר לטלוויזיה, התקן “File Manager” מהחנות, פתח את ה-APK והתקן.

### דרך 3 — ADB (למתקדמים)
1. בטלוויזיה: הגדרות → אודות → הקש על “Build” כמה פעמים כדי להפעיל “מצב מפתח”, והפעל **USB/Network debugging**.
2. מהמחשב:
```bash
adb connect <כתובת_IP_של_הטלוויזיה>
adb install -r app-debug.apk
```

---

## שלב 4 — הרצה וחיבור
1. פתח את “IPTV נגן”.
2. “+ הוספת פרופיל / שרת חדש” → שרת: `http://line.trex4k.top`, ומלא שם משתמש וסיסמה.
3. “התחבר וטען רשימה”. באפליקציה החיבור הישיר אמור לעבוד (אין CORS / Mixed-Content).

---

## שליטה בשלט רחוק (טלוויזיה)
הנגן תומך בחיצי מקלדת/שלט (מעלה/מטה למעבר בין ערוצים, Enter לבחירה, רווח לנגינה/השהיה).
אם הניווט בשלט אינו נוח די—אפשר לחבר עכבר Bluetooth/USB לטלוויזיה לניווט נוח יותר.

## רקומה 513 / חסימת User-Agent
אם החיבור נכשל גם באפליקציה — פתח את `MainActivity.java` והגדר:
```java
private static final String CUSTOM_USER_AGENT = "VLC/3.0.18 LibVLC/3.0.18";
```
ובנה מחדש.

## עדכון תוכן הנגן
החלף את `app/src/main/assets/index.html` ובנה מחדש.
