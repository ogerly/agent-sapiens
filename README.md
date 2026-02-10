# Agent Sapiens 🧠

> **Formbare Gedächtnisarchitektur für intelligente Agenten**  
> *Von der Datenspeicherung zur kognitiven Morphogenese*
<p align="center">
  <img src="image.png" alt="Agent Sapiens – Architektur-Visualisierung: Schichtenmodell mit L1 (Episodisch), L2 (Assoziativ), L3 (Konsolidiert), Promotion Gates, TLTM/HLTM-Trennung und das Taschenlampen-Problem" width="800" />
</p>


[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Status](https://img.shields.io/badge/status-concept-orange.svg)]()
[![Version](https://img.shields.io/badge/version-2.0-green.svg)]()

---

## 🎯 Vision

**Agent Sapiens** ist kein KI-Modell. Es ist ein **architektonisches Paradigma** für Gedächtnissysteme, die nicht nur speichern, sondern *reifen*.

Statt immer größerer Datenberge entwickeln Agenten:
- **Räumliche Orientierung** statt bloßer Faktensammlung
- **Strukturelle Intelligenz** statt Rohdaten-Retrieval
- **Meta-Kognition** über eigene Wissensgrenzen
- **Persönliche Beziehungen** zu ihren Nutzern

### Der Unterschied

| Klassische RAG-Systeme | Agent Sapiens |
|------------------------|---------------|
| "Speichere alles" | "Konsolidiere das Wichtige" |
| Vektor-Datenbank | Formbare Schicht-Architektur |
| Statisches Retrieval | Dynamische Gedächtnisbildung |
| Keine Unsicherheit | "Ich weiß nicht" als Feature |
| Ein Gedächtnis für alles | Dual Memory (Technisch vs. Human) |

---

## 📚 Dokumentation

Dieses Repository enthält die vollständige konzeptionelle und technische Dokumentation:

### Kern-Dokumente

1. **[Agent Sapiens Blueprint](./docs/agent_sapiens_blueprint.md)**  
   *Die konzeptionelle Fundierung* – Warum brauchen wir das?
   - Das Taschenlampen-Problem
   - Duale Gedächtnisarchitektur (TLTM/HLTM)
   - Layer-Modell (L1-L5)
   - Memory Morphogenesis
   - Spatial/Structural Memory

2. **[Minimal Viable Implementation](./docs/agent_sapiens_mvs_implementation.md)**  
   *Der pragmatische Fahrplan* – Was können wir in 6 Monaten bauen?
   - Reduzierung auf 3 Layers
   - Tech-Stack (SQLite + ChromaDB)
   - Inkrementelle Konsolidierung
   - 4-Wochen-MVP-Plan

3. **[Final Synthesis](./docs/agent_sapiens_final_synthesis.md)**  
   *Die wissenschaftliche Verfeinerung* – Wie machen wir es richtig?
   - Verbesserte Domain-Klassifikation (Multi-Modal)
   - Wissenschaftlich fundiertes Decay-Modell
   - Learning Velocity & Transfer Learning
   - Community-Integration

---

## 🏗️ Architektur-Überblick

### Dual Memory System

```
┌─────────────────────────────────────────────────┐
│           Agent Sapiens Memory                  │
├─────────────────────┬───────────────────────────┤
│  TLTM                │  HLTM                     │
│  (Technical)         │  (Human-Relational)       │
├─────────────────────┼───────────────────────────┤
│  • Fakten            │  • Präferenzen            │
│  • Code-Muster       │  • Beziehungsmuster       │
│  • API-Strukturen    │  • Tonalität              │
│  • Best Practices    │  • Narrative              │
│                      │                           │
│  Konsolidiert        │  Kultiviert               │
│  Konfliktauflösend   │  Widerspruchsoffen        │
└─────────────────────┴───────────────────────────┘
```

### Layer-Architektur

```
┌─────────────────────────────────────┐
│  L3: Consolidated (Regeln)          │  ← Validiert, stabil
│      "User bevorzugt Tabs"          │
├─────────────────────────────────────┤
│  L2: Working (Assoziationen)        │  ← Aktiv genutzt
│      "Python ⟷ Data Science"        │
├─────────────────────────────────────┤
│  L1: Episodic (Roh-Events)          │  ← Flüchtig, 7 Tage
│      "User fragte nach asyncio"     │
└─────────────────────────────────────┘
           ↑
    Promotion Gates
    (Frequency, Recency, Impact)
```

---

## 🚀 Schnellstart

### Voraussetzungen

```bash
Python >= 3.10
SQLite >= 3.35
16GB RAM (empfohlen)
```

### Installation (Coming Soon)

```bash
# Repository klonen
git clone https://github.com/yourusername/agent-sapiens.git
cd agent-sapiens

# Virtuelle Umgebung erstellen
python -m venv venv
source venv/bin/activate  # Linux/Mac
# oder
venv\Scripts\activate  # Windows

# Dependencies installieren
pip install -r requirements.txt

# Setup
python setup.py install
```

### Beispiel-Nutzung

```python
from agent_sapiens import MemorySystem, Agent

# Memory-System initialisieren
memory = MemorySystem(
    storage_path="./my_agent_memory",
    config={
        'layers': 3,
        'decay_half_life_days': 30,
        'promotion_gates': {
            'l1_to_l2': {'frequency': 2, 'recency_days': 7},
            'l2_to_l3': {'frequency': 5, 'confidence': 0.7}
        }
    }
)

# Agent erstellen
agent = Agent(memory=memory, specialization="python_coding")

# Interaktion
agent.remember("Ich bevorzuge List Comprehensions", domain="HLTM")
agent.remember("Flask API: POST /api/users erstellt User", domain="TLTM")

# Abrufen
knowledge = agent.recall("Wie erstelle ich einen User?")
# → Gibt: "POST /api/users" (aus TLTM)

# Meta-Wissen
insight = agent.assess_knowledge("Flask API")
# → {'coverage': 0.45, 'confidence': 0.7, 'uncertainties': [...]}
```

---

## 🎓 Konzepte verstehen

### 1. Das Taschenlampen-Problem

Klassische LLMs operieren mit einem begrenzten Context Window – wie eine Taschenlampe im dunklen Raum. **Agent Sapiens** baut eine *mentale Landkarte* des Raumes auf.

```
Klassisch:  🔦 → Sehe nur aktuelle Tokens
Sapiens:    🗺️ → Habe Karte des gesamten Wissensraums
```

### 2. Dual Memory (TLTM vs. HLTM)

**Technisches Gedächtnis (TLTM):**
- Fakten, Code, APIs
- Widersprüche werden aufgelöst
- "Die API ist jetzt v2, nicht mehr v1"

**Humanes Gedächtnis (HLTM):**
- Präferenzen, Beziehungen, Stil
- Widersprüche werden *gehalten*
- "User mag formale Sprache bei Arbeit, casual privat"

### 3. Promotion Gates

Nicht jede Information steigt ins Langzeitgedächtnis auf:

```python
def promote_to_l2(episode):
    if episode.frequency >= 2 and \
       episode.recency < 7_days and \
       episode.impact > threshold:
        return True
    return False
```

### 4. Spatial Memory

Agent speichert nicht nur *was*, sondern *wo* im inneren Raum:

```
Topic: "Python"
├─ Cluster: "async programming"
│  ├─ asyncio [high-energy]
│  ├─ await/async [medium-energy]
│  └─ event loop [low-energy]
└─ Cluster: "data science"
   └─ pandas [high-energy]
```

---

## 📊 Use Case: Python-Code-Assistent

Der erste validierte Use Case ist ein **persönlicher Python-Programmier-Assistent**.

### Was lernt er?

**Technisch (TLTM):**
- ✅ Deine häufigsten Fehler
- ✅ APIs, die du nutzt
- ✅ Best Practices, die funktionieren

**Persönlich (HLTM):**
- ✅ Deinen Code-Stil (Tabs vs. Spaces)
- ✅ Deine Naming-Conventions
- ✅ Deine Kommunikations-Präferenzen

### Messbarer Erfolg

Nach 30 Tagen:
- **85%** Stil-Konsistenz
- **30%** Fehler-Reduktion
- **>90%** Präferenz-Genauigkeit

---

## 🛠️ Entwicklungs-Roadmap

### Phase 1: Foundation ✅ (Wochen 1-4)
- [x] Konzeptionelle Dokumentation
- [ ] SQLite + ChromaDB Setup
- [ ] Basis-Layer-System
- [ ] Domain-Klassifikation

### Phase 2: Spezialisierung 🔄 (Wochen 5-8)
- [ ] Python-spezifisches Schema
- [ ] Code-Stil-Lernen
- [ ] Error-Pattern-Erkennung
- [ ] Alpha-Test mit 5 Usern

### Phase 3: Transparenz 📋 (Wochen 9-12)
- [ ] Memory-Dashboard UI
- [ ] Explain-Funktion
- [ ] User-Control-API
- [ ] Privacy-Layer

### Phase 4: Metriken 📈 (Wochen 13-16)
- [ ] Learning Velocity
- [ ] Transfer Learning
- [ ] Error Recovery
- [ ] A/B-Testing-Framework

### Phase 5: Community 🌐 (Wochen 17-20)
- [ ] Pattern-Validation
- [ ] Crowdsourced Tests
- [ ] Public Beta

### Phase 6: Production 🚀 (Wochen 21-26)
- [ ] Performance-Optimierung
- [ ] Skalierung (>10k Episoden)
- [ ] Dokumentation
- [ ] v1.0 Release

---

## 🧪 Wissenschaftliche Grundlagen

Agent Sapiens basiert auf etablierter Gedächtnis-Forschung:

- **Spacing-Effekt** (Cepeda et al., 2006): Wiederholungen über Zeit verstärken Gedächtnis
- **Ebbinghaus-Kurve** (1885): Vergessen folgt Power-Law, nicht Exponentialfunktion
- **Dual-Process-Theorie** (Kahneman): Schnelles vs. langsames Denken
- **Cognitive Maps** (Tolman, 1948): Räumliche Repräsentation von Wissen

### Wichtige Paper

1. *MemGPT: Towards LLMs as Operating Systems* (Packer et al., 2023)
2. *The Spacing Effect in Learning* (Cepeda et al., 2006)
3. *Memory Consolidation During Sleep* (Stickgold, 2005)

---

## 🤝 Beitragen

Wir suchen:
- **Forscher** für empirische Validierung
- **Entwickler** für Implementierung
- **Designer** für UX/UI
- **Beta-Tester** für Use Cases

### Contribution Guidelines

1. **Forke** das Repository
2. **Erstelle** einen Feature-Branch (`git checkout -b feature/amazing-feature`)
3. **Committe** deine Änderungen (`git commit -m 'Add amazing feature'`)
4. **Pushe** zum Branch (`git push origin feature/amazing-feature`)
5. **Öffne** einen Pull Request

### Code of Conduct

Wir folgen dem [Contributor Covenant](https://www.contributor-covenant.org/).  
Respektvoller Umgang, konstruktives Feedback, inklusive Community.

---

## 📈 Metriken & Erfolg

### Quantitative Ziele (nach 6 Monaten)

| Metrik | Ziel | Status |
|--------|------|--------|
| Domain-Klassifikation | >95% Genauigkeit | 🔄 In Arbeit |
| Stil-Konsistenz (Python) | >85% | 📋 Geplant |
| Fehler-Reduktion | >30% | 📋 Geplant |
| Retention (30 Tage) | >85% | 📋 Geplant |
| User-Satisfaction | >4.0/5.0 | 📋 Geplant |

### Qualitative Erfolge

- ✅ Agent kann sagen "Ich bin mir nicht sicher"
- ✅ User verstehen, was gespeichert wird
- ✅ Lokale Ausführung ohne Cloud
- ✅ Community-getriebene Verbesserung

---

## 🔐 Privacy & Ethik

### Unsere Prinzipien

1. **Privacy-by-Design**  
   Alle Daten lokal, verschlüsselt, unter User-Kontrolle

2. **Transparenz**  
   User sehen, was gespeichert wird und warum

3. **Kontrolle**  
   Explizites Vergessen, Korrigieren, Exportieren

4. **Fairness**  
   Bias-Detection, keine diskriminierenden Muster

5. **Safety**  
   Keine Speicherung schädlicher Inhalte

### GDPR-Konformität

- ✅ Recht auf Löschung
- ✅ Recht auf Daten-Export
- ✅ Recht auf Berichtigung
- ✅ Minimale Datenspeicherung
- ✅ Transparente Verarbeitung

---

## 🌟 Community

### Diskussionen

- **Discord:** [Link folgt]
- **Forum:** [Link folgt]
- **GitHub Discussions:** [Hier](https://github.com/yourusername/agent-sapiens/discussions)

### Regelmäßige Calls

- **Montags, 18:00 UTC:** Research Review
- **Mittwochs, 19:00 UTC:** Technical Deep-Dive
- **Freitags, 17:00 UTC:** Community Office Hours

---

## 📄 Lizenz

Dieses Projekt steht unter der **MIT-Lizenz** – siehe [LICENSE](LICENSE) für Details.

### Warum MIT?

Wir glauben an:
- Offene Forschung
- Demokratisierung von KI
- Community-getriebene Innovation
- Keine proprietären Lock-ins

---

## 🙏 Danksagungen

Dieses Projekt wurde inspiriert durch:

- **Gemini** (Google DeepMind) für initiale Konzeptualisierung
- **DeepSeek** für wissenschaftliche Kritik und Verfeinerung
- **Claude** (Anthropic) für architektonische Synthese
- Die **OpenClaw-Community** für Vision und Feedback

Besonderer Dank an alle, die an der Konzeption mitgewirkt haben.

---

## 📞 Kontakt

- **Projekt-Lead:** [Dein Name]
- **Email:** [deine@email.com]
- **Twitter/X:** [@handle]
- **Website:** [website.com]

---

## 🔮 Vision Statement

> "Intelligenz ist nicht die Menge gespeicherter Daten,  
> sondern die Qualität der inneren Karte."

**Agent Sapiens** zeigt, dass KI-Agenten:
- Lokal statt zentralisiert sein können
- Persönlich statt generisch sein können
- Respektvoll statt invasiv sein können
- Reif statt nur groß sein können

Wir bauen keine bessere Datenbank.  
Wir bauen ein **neues Paradigma** für kognitive Systeme.

---

<div align="center">

**🚀 Die Revolution beginnt nicht mit dem perfekten System.**  
**Sie beginnt mit dem ersten funktionierenden Prototyp.**

[⭐ Star dieses Projekt](https://github.com/yourusername/agent-sapiens) • 
[📖 Dokumentation](./docs/) • 
[🐛 Issues](https://github.com/yourusername/agent-sapiens/issues) • 
[💬 Diskussionen](https://github.com/yourusername/agent-sapiens/discussions)

</div>

---

*Made with 🧠 by the Agent Sapiens Community*  
*Last updated: Februar 2026*






