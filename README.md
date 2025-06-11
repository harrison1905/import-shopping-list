# recipe-import

Ein Scriptable-Workflow, um Einkaufslisten aus Cookidoo direkt in Apple Erinnerungen zu übernehmen.

## 📋 Projektbeschreibung

Mit diesem Repository kannst du:

* Aus Cookidoo per **Teilen**-Funktion den Einkaufslisten-Text an **Scriptable** übergeben.
* Scriptable filtert die Zeilen mit Mengenangaben heraus und erstellt daraus einzelne Erinnerungen in der Liste **„Einkäufe“**.
* Du sparst Zeit und behältst automatisch alle Einkaufspunkte im Blick.

## 🚀 Features

* **Automatische Zeilenfilterung**: Nur Zeilen, die mit einer Ziffer beginnen, werden als Einkaufspunkte erkannt.
* **Erinnerungsliste anlegen**: Falls noch nicht vorhanden, wird eine Erinnerungs-Liste namens **„Einkäufe“** erstellt.
* **Nahtlose Integration**: Einfach aus Cookidoo in der Teilen-Ansicht auswählen.


## 🛠️ Installation
      https://scriptable.app/
      
      importiere Cookidoo.scriptable 

## 📜 Scriptable-Skript

Pfad: `scriptable/einkaufsliste.js`

```javascript
// Variables used by Scriptable.
// icon-color: blue; icon-glyph: list;

// 1. Text einlesen (aus Share Sheet oder Clipboard)
let text = "";
if (args.plainTexts && args.plainTexts.length > 0) {
  text = args.plainTexts.join("\n");
} else {
  text = Pasteboard.general.string;
}
if (!text) {
  console.error("Kein Text gefunden.");
  return;
}

// 2. Zeilen splitten und filtern (nur Zeilen, die mit Zahl beginnen)
let lines = text.split(/\r?\n/);
let items = lines
  .map(l => l.trim())
  .filter(l => /^\d/.test(l));

if (items.length === 0) {
  console.log("Keine passenden Einkaufs­punkte gefunden.");
  return;
}

// 3. Erinnerungs­liste finden oder anlegen
let listTitle = "Einkäufe";
let list = await Calendar.findOrCreateForReminders(listTitle);

// 4. Für jeden Punkt eine neue Erinnerung anlegen
for (let item of items) {
  let reminder = new Reminder();
  reminder.title = item;
  reminder.calendar = list;
  reminder.save();
}

// Script beenden
Script.complete();
```

## 🤝 Mitwirken

Beiträge sind willkommen! Erstelle gerne Pull Requests für neue Features oder Berichte Bugs über Issues.

## 📄 Lizenz

Dieses Projekt steht unter der MIT-Lizenz. Siehe [LICENSE](LICENSE) für Details.
