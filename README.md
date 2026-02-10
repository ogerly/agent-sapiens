# Agent Sapiens

**Eine neue Art, wie Agenten sich erinnern.**

<p align="center">
  <img src="image.png" alt="Agent Sapiens – Architektur-Visualisierung: Schichtenmodell mit L1 (Episodisch), L2 (Assoziativ), L3 (Konsolidiert), Promotion Gates, TLTM/HLTM-Trennung und das Taschenlampen-Problem" width="800" />
</p>

---

## Worum geht es hier?

Stell dir vor, du redest jeden Tag mit einem Agenten. Du erzählst ihm, dass du Python liebst, dass du Tabs statt Spaces bevorzugst, dass du Horror-Filme eigentlich nicht magst – außer *Hereditary*, der war großartig. Du erklärst ihm dreimal, wie dein Projekt aufgebaut ist. Und beim vierten Mal? Fragt er wieder von vorne.

Das ist der Zustand heute. Agenten haben schlicht keine Ahnung – kein echtes Gedächtnis, kein Verständnis davon, was relevant ist. Ihre Lösung? Alles Mögliche in den Prompt packen. Konversationshistorie, Dokumente, Notizen – ungefiltert, unstrukturiert, auf Masse statt auf Klugheit. Das Problem verschiebt sich dann nur: Das LLM ertrinkt im Kontext. Zu viel, zu wahllos, zu flach. Der Lichtkegel der Taschenlampe wird breiter, aber nicht heller.

**Agent Sapiens ist der Versuch, das zu ändern.**

Nicht durch größere Prompts. Nicht durch längere Kontextfenster. Sondern durch eine Architektur, die dem nachempfunden ist, wie Gedächtnis tatsächlich funktioniert: als formbares, lebendiges System, das lernt, vergisst, sortiert und reift – und dem LLM nur das gibt, was es wirklich braucht.

---

## Der Gedankengang

<p align="center">
  <img src="agents.png" alt="Evolution der Agenten-Typen – vom einfachen Daten-Agenten zum Agent Sapiens mit gereiftem Gedächtnis" width="800" />
</p>

Die Idee hinter Agent Sapiens entstand aus einer einfachen Beobachtung: **Menschen erinnern sich nicht an alles – und genau das macht ihr Gedächtnis so leistungsfähig.** Wir vergessen das Unwichtige, verdichten Erfahrungen zu Mustern, und navigieren mit einer inneren Landkarte durch Themen, statt jedes Detail nachzuschlagen.

Aktuelle Agenten tun das Gegenteil. Sie wissen nichts über dich und stopfen stattdessen alles in den Prompt – in der Hoffnung, dass das LLM schon das Richtige daraus fischt. Sie behandeln eine technische Fehlermeldung genauso wie eine persönliche Stilpräferenz. Sie lösen Widersprüche auf, statt sie als Information zu bewahren. Und das LLM? Verliert den Faden, weil ihm zu viel und zu wenig Kluges auf einmal vorgesetzt wird.

Agent Sapiens stellt drei Fragen:

1. **Was wäre, wenn ein Agent lernt, was wichtig ist – und den Rest vergisst?**
2. **Was wäre, wenn er technisches Wissen anders behandelt als menschliches?**
3. **Was wäre, wenn er weiß, was er nicht weiß?**

Aus diesen Fragen ist ein Architektur-Entwurf geworden, ein pragmatischer Implementierungsplan, und schließlich eine Synthese, die beides zusammenführt.

---

## Die drei Dokumente

Dieses Projekt besteht aus drei Dokumenten, die aufeinander aufbauen – von der Vision zur Umsetzung:

### 1. Der Blueprint – *Die Vision*
📄 [Blueprint.md](Blueprint.md)

Das Grundsatzdokument. Warum braucht ein Agent zwei getrennte Gedächtnissysteme? Wie funktioniert das Aufsteigen von Erinnerungen durch Schichten? Was bedeutet es, wenn ein Agent nicht Fakten speichert, sondern Räume kartiert?

**Kernideen:**
- **Duales Gedächtnis:** Technisches Wissen (TLTM) und menschliches Wissen (HLTM) werden grundverschieden behandelt. Widersprüche im menschlichen Bereich sind kein Fehler, sondern Information.
- **Schichtenmodell:** Erinnerungen steigen von rohen Episoden über Assoziationen zu konsolidierten Regeln auf – durch Filter, die Relevanz prüfen.
- **Räumliches Gedächtnis:** Der Agent speichert nicht nur *was*, sondern *wo* im inneren Wissensraum etwas liegt. Er navigiert statt zu suchen.
- **Reife statt Größe:** Nicht die Datenmenge zählt, sondern die Qualität der inneren Karte.

### 2. Die Minimal Viable Implementation – *Der Pragmatismus*
📄 [Minimal Viable Implementation.md](Minimal%20Viable%20Implementation.md)

Was davon kann man tatsächlich bauen? Dieses Dokument übersetzt die Vision in einen 6-Monats-Plan, der auf einem normalen Laptop läuft. Kein GPU nötig, keine Server-Infrastruktur.

**Kernentscheidungen:**
- 5 Schichten werden auf 3 reduziert (Episodisch → Arbeitsgedächtnis → Konsolidiert)
- SQLite + ChromaDB statt verteilter Datenbanken
- Inkrementelle Konsolidierung statt großer Batch-Schlafphasen
- Einfache, regelbasierte Promotion Gates als Startpunkt
- Fokus auf einen einzelnen Agenten, kein Multi-Agent-Transfer

### 3. Die Finale Synthese – *Die Exzellenz*
📄 [Agent Sapiens Finale Synthese.md](Agent%20Sapiens%20Finale%20Synthese.md)

Die Zusammenführung. Hier werden die Schwächen des pragmatischen Plans identifiziert und durch wissenschaftlich fundiertere Ansätze ersetzt. Gleichzeitig wird ein konkreter Killer-Use-Case definiert: ein persönlicher Python-Programmier-Assistent, der deinen Code-Stil lernt.

**Was hier dazukommt:**
- Embedding-basierte Domain-Klassifikation statt simpler Wortlisten
- Wissenschaftlich fundiertes Vergessen (Spacing-Effekt, Power-Law statt naivem Exponential-Decay)
- Mehrdimensionale Wissenstiefe statt einfachem Konzept-Zählen
- Transparenz-UI: Der Agent zeigt, was er weiß, wo er unsicher ist, und warum
- Volle Nutzerkontrolle: Vergessen, Korrigieren, Lehren
- Privacy-by-Design: Lokale Speicherung, Verschlüsselung, GDPR-konform

---

## Die Konzepte im Überblick

### Das Taschenlampen-Problem

Agenten heute haben keinen Überblick. Sie leuchten mit einem schmalen Lichtkegel auf den aktuellen Prompt – und alles drumherum ist dunkel. Ihre Lösung ist, den Kegel größer zu machen, indem sie mehr und mehr Kontext reinpacken. Aber ein breiterer Kegel ist nicht dasselbe wie eine Karte des Raumes.

Agent Sapiens baut eine innere Landkarte auf. Der Agent weiß nicht nur, was gerade beleuchtet ist, sondern wo alles andere liegt.

### Duales Gedächtnis (TLTM vs. HLTM)

Nicht alles Wissen ist gleich. Technisches Wissen ("Die API ist jetzt v2") und menschliches Wissen ("Du magst formale Sprache bei der Arbeit, aber lockere in privaten Chats") funktionieren fundamental anders.

**Technisches Gedächtnis (TLTM):** Widersprüche werden aufgelöst. Das Neuere ersetzt das Alte.

**Humanes Gedächtnis (HLTM):** Widersprüche werden *gehalten*. Du magst keine Horror-Filme, aber Hereditary war großartig – beides stimmt. Das ist kein Bug, sondern Nuance.

### Schichtenmodell

Erinnerungen beginnen als flüchtige Episoden und können durch **Promotion Gates** aufsteigen:

```
L3: Konsolidiert       ← Validierte Regeln & Muster (dauerhaft)
L2: Arbeitsgedächtnis  ← Häufige Assoziationen (90 Tage)
L1: Episodisch         ← Rohe Erlebnisse (7 Tage)
```

Nicht alles steigt auf. Nur was wiederholt vorkommt, relevant ist und bestätigt wird. Der Rest verblasst – genau wie beim Menschen.

### Spatial Memory

Der Agent speichert nicht nur *was*, sondern *wo* im inneren Wissensraum etwas liegt:

```
Thema: Python
├── Cluster: async programming
│   ├── asyncio [oft genutzt]
│   └── event loop [selten]
└── Cluster: data science
    └── pandas [oft genutzt]
```

Das ermöglicht Navigation statt Suche. Und der Agent kennt seine Coverage – er weiß, welche Bereiche gut kartiert sind und wo Lücken klaffen.

---

## Der Use Case: Persönlicher Python-Assistent

Der erste geplante Anwendungsfall ist ein persönlicher Python-Programmier-Assistent, der über die Zeit lernt:

**Technisch:** Deine häufigsten Fehler, die APIs die du nutzt, Best Practices die für dich funktionieren.

**Persönlich:** Deinen Code-Stil, deine Naming-Conventions, wie du Kommunikation bevorzugst.

Ziel nach 30 Tagen: Der Agent generiert Code, der aussieht wie *deiner* – nicht wie ein generisches Beispiel aus dem Internet.

---

## Wissenschaftliche Grundlagen

Die Konzepte basieren auf etablierter Gedächtnis-Forschung:

- **Ebbinghaus-Kurve** (1885): Vergessen folgt einer Power-Law-Kurve, nicht einer Exponentialfunktion
- **Spacing-Effekt** (Cepeda et al., 2006): Wiederholungen über Zeit verteilt verstärken Gedächtnis mehr als geballte Wiederholung
- **Dual-Process-Theorie** (Kahneman): Schnelles vs. langsames Denken – die Grundlage für die TLTM/HLTM-Trennung
- **Cognitive Maps** (Tolman, 1948): Räumliche Repräsentation von Wissen im Gedächtnis

Technische Referenzen: MemGPT (Packer et al., 2023), LangChain Memory, ChromaDB.

---

## Status

Dieses Projekt befindet sich in der **konzeptionellen Phase**. Die Dokumente beschreiben Gedanken, Architektur-Entwürfe und einen Implementierungsplan. Code-Beispiele in den Dokumenten sind illustrativ – es existiert noch keine lauffähige Software.

---

## Danksagungen

Dieses Projekt wurde inspiriert und begleitet durch:

- **Gemini** (Google DeepMind) für die initiale Konzeptualisierung
- **DeepSeek** für wissenschaftliche Kritik und Verfeinerung
- **Claude** (Anthropic) für die architektonische Synthese
- Die **OpenClaw-Community** für Vision und Feedback

---

*Agent Sapiens ist kein Produkt. Es ist ein Gedanke, der darauf wartet, gebaut zu werden.*
