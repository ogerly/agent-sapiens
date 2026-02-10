# Agent Sapiens: Minimal Viable Implementation
## Pragmatischer Fahrplan für OpenClaw

**Basierend auf:** Agent Sapiens Blueprint v1.0  
**Fokus:** Machbarkeit, Iteration, empirische Validierung  
**Zeitrahmen:** 6-12 Monate bis zum funktionalen Prototyp  
**Hardware-Ziel:** Consumer-Laptop (16GB RAM, keine GPU erforderlich)

---

## Kernprinzip: Radikal vereinfachen, Essenz bewahren

**Was wir BEHALTEN:**
1. ✅ Duale Gedächtnisarchitektur (TLTM/HLTM)
2. ✅ Spatial/Structural Memory als Orientierungssystem
3. ✅ Promotion Gates für selektives Gedächtnis
4. ✅ Meta-Wissen über eigene Unsicherheit

**Was wir STREICHEN (für v1):**
1. ❌ 5-Layer-Hierarchie → Reduziert auf 3 Layers
2. ❌ Komplexe Batch-Konsolidierung → Inkrementelle Updates
3. ❌ Agent-zu-Agent-Transfer → Fokus auf Single-Agent
4. ❌ Ausgefeiltes Konfliktmanagement → Einfache Versionierung

---

## Phase 1: Foundation (Monat 1-2)

### Architektur-Entscheidungen

#### Reduziertes Layer-Modell
```
┌─────────────────────────────────┐
│  L3: Consolidated (Regeln)      │  ← Konsolidiert, stabil
├─────────────────────────────────┤
│  L2: Working (Assoziationen)    │  ← Aktiv genutzt
├─────────────────────────────────┤
│  L1: Episodic (Roh-Events)      │  ← Flüchtig, 7-Tage-Retention
└─────────────────────────────────┘
```

**Begründung:**  
- L1: Kurzfristige Konversationsgeschichte
- L2: Thematische Cluster und häufige Muster
- L3: Validierte Präferenzen und Regeln

#### Tech-Stack (minimal)

```yaml
Persistierung:
  L1: SQLite mit JSON-Feldern (einfach, portabel)
  L2: Embedded Vector-DB (ChromaDB, in-process)
  L3: SQLite Relations-Tabellen
  
Processing:
  Live: Synchron (kein extra Stream-Processing)
  Async: Python AsyncIO für Background-Tasks
  
LLM:
  Lokal: llama.cpp mit kleinem Modell für Embeddings
  Cloud: Claude API für Abstraktion (optional, nur bei WiFi)
```

**Warum SQLite?**  
- Keine Installation
- Portabel (Single-File)
- Transaktional
- Ausreichend für <1M Einträge

### Daten-Modell (vereinfacht)

```python
# L1: Episodic Memory
class Episode:
    id: str
    timestamp: datetime
    content: str
    context_tags: List[str]
    user_id: str
    saliency: float = 1.0
    access_count: int = 0
    last_accessed: datetime
    
# L2: Working Memory (Assoziativ)
class Association:
    id: str
    concept_a: str
    concept_b: str
    strength: float  # 0.0 - 1.0
    evidence_count: int
    last_reinforced: datetime
    domain: str  # "TLTM" oder "HLTM"
    
# L3: Consolidated Memory
class ConsolidatedKnowledge:
    id: str
    pattern_type: str  # "preference", "rule", "fact"
    content: dict
    confidence: float
    source_episodes: List[str]  # Traceability
    domain: str  # "TLTM" oder "HLTM"
    created_at: datetime
    validated: bool = False
```

### Minimale Promotion Gates

```python
class PromotionGates:
    """Einfache, regelbasierte Gates"""
    
    THRESHOLDS = {
        'L1_to_L2': {
            'frequency': 2,      # Mind. 2x erwähnt
            'recency_days': 7    # In letzten 7 Tagen
        },
        'L2_to_L3': {
            'frequency': 5,      # Mind. 5x bestätigt
            'confidence': 0.7    # Mind. 70% Konfidenz
        }
    }
    
    def check_L1_to_L2(self, episode: Episode) -> bool:
        """Darf Episode zu L2 aufsteigen?"""
        similar = self.find_similar_episodes(episode)
        
        return (
            len(similar) >= self.THRESHOLDS['L1_to_L2']['frequency'] and
            self.recency_check(similar, days=7)
        )
    
    def check_L2_to_L3(self, association: Association) -> bool:
        """Darf Assoziation zu L3 konsolidiert werden?"""
        return (
            association.evidence_count >= self.THRESHOLDS['L2_to_L3']['frequency'] and
            association.strength >= self.THRESHOLDS['L2_to_L3']['confidence']
        )
```

---

## Phase 2: Spatial Memory (Monat 3-4)

### Vereinfachte Raum-Metapher

**Konzept:**  
Statt komplexer Topologie → einfache **Themen-Cluster** mit Distanzen.

```python
class ConceptSpace:
    """Vereinfachtes räumliches Gedächtnismodell"""
    
    def __init__(self):
        self.clusters = {}  # topic -> List[concepts]
        self.embeddings = {}  # concept -> vector
        self.distances = {}  # (concept_a, concept_b) -> distance
        
    def add_concept(self, concept: str, topic: str):
        """Füge Konzept zu Raum hinzu"""
        embedding = self.embed(concept)
        self.embeddings[concept] = embedding
        
        if topic not in self.clusters:
            self.clusters[topic] = []
        self.clusters[topic].append(concept)
        
    def find_nearby(self, concept: str, radius: float = 0.3) -> List[str]:
        """Finde semantisch nahe Konzepte"""
        base_vec = self.embeddings[concept]
        nearby = []
        
        for other_concept, other_vec in self.embeddings.items():
            distance = cosine_distance(base_vec, other_vec)
            if distance < radius:
                nearby.append(other_concept)
                
        return nearby
    
    def assess_coverage(self, topic: str) -> float:
        """Wie gut kennen wir dieses Thema?"""
        if topic not in self.clusters:
            return 0.0
            
        # Einfache Heuristik: Anzahl Konzepte normalisiert
        concept_count = len(self.clusters[topic])
        return min(1.0, concept_count / 20)  # Max bei 20 Konzepten
```

### Meta-Wissen (vereinfacht)

```python
class MemoryInsight:
    """Was weiß der Agent über sein eigenes Wissen?"""
    
    def __init__(self, memory_system):
        self.memory = memory_system
        
    def assess_knowledge(self, topic: str) -> dict:
        """Selbsteinschätzung zu einem Thema"""
        
        # Wie viele Episoden?
        episodes = self.memory.l1.get_by_topic(topic)
        
        # Wie viele konsolidierte Fakten?
        consolidated = self.memory.l3.get_by_topic(topic)
        
        # Coverage im Raum
        coverage = self.memory.spatial.assess_coverage(topic)
        
        return {
            'coverage': coverage,
            'episode_count': len(episodes),
            'consolidated_count': len(consolidated),
            'confidence': self._calculate_confidence(episodes, consolidated),
            'last_update': max([e.timestamp for e in episodes], default=None)
        }
    
    def can_answer_confidently(self, topic: str, threshold: float = 0.6) -> bool:
        """Sollte der Agent eine Antwort geben oder Unsicherheit signalisieren?"""
        assessment = self.assess_knowledge(topic)
        return assessment['confidence'] > threshold
```

**Nutzen:**  
Agent kann sagen: *"Ich habe nur 3 Datenpunkte zu X, bin mir also nicht sicher."*

---

## Phase 3: TLTM/HLTM Separation (Monat 3-5)

### Praktische Implementierung der Trennung

```python
class DualMemorySystem:
    """Trennung von technischem und humanem Gedächtnis"""
    
    def __init__(self):
        self.tltm = TechnicalMemory()
        self.hltm = HumanMemory()
        
    def store(self, content: str, domain: str, context: dict):
        """Router: Wohin gehört diese Information?"""
        
        if self._is_technical(content, context):
            self.tltm.store(content, allow_override=True)
        else:
            self.hltm.store(content, preserve_nuance=True)
    
    def _is_technical(self, content: str, context: dict) -> bool:
        """Heuristik: Technisch vs. Human"""
        
        technical_indicators = [
            'error', 'code', 'function', 'api', 'syntax',
            'compile', 'debug', 'library', 'version'
        ]
        
        human_indicators = [
            'prefer', 'like', 'feel', 'think', 'want',
            'mood', 'style', 'tone', 'relationship'
        ]
        
        tech_score = sum(1 for word in technical_indicators if word in content.lower())
        human_score = sum(1 for word in human_indicators if word in content.lower())
        
        return tech_score > human_score

class TechnicalMemory:
    """TLTM: Konsolidiert, konfliktauflösend"""
    
    def store(self, content: str, allow_override: bool = True):
        # Prüfe Widerspruch
        existing = self.find_related(content)
        
        if existing and allow_override:
            # Technisches Wissen: Überschreibe bei Konflikt
            self.update(existing.id, content, reason="newer_version")
        else:
            self.create_new(content)

class HumanMemory:
    """HLTM: Kultiviert, widerspruchsoffen"""
    
    def store(self, content: str, preserve_nuance: bool = True):
        # Prüfe Widerspruch
        existing = self.find_related(content)
        
        if existing and preserve_nuance:
            # Humanes Wissen: HALTE beide Varianten
            self.add_variant(existing.id, content, context=self.extract_context())
        else:
            self.create_new(content)
    
    def add_variant(self, base_id: str, variant: str, context: dict):
        """Speichere kontextuelle Variante"""
        # Beispiel: User mag formale Sprache im Job, casual in Freizeit
        self.variants[base_id].append({
            'content': variant,
            'context': context,
            'timestamp': datetime.now()
        })
```

### Konflikt-Beispiel (konkret)

```python
# Szenario: User sagt widersprüchliches
interaction_1 = "Ich mag keine Horror-Filme"
interaction_2 = "Hereditary war großartig"  # Hereditary IST ein Horror-Film

# TLTM würde überschreiben (Konflikt auflösen)
# HLTM hält beide:

hltm.store("genre_preference: horror = negative", context={'general': True})
hltm.store("movie_rating: Hereditary = positive", context={'specific': True})

# Resultat im HLTM:
{
    "concept": "horror_movies",
    "variants": [
        {"opinion": "dislike", "context": "general", "confidence": 0.7},
        {"opinion": "like", "context": "specific_film", "confidence": 0.9}
    ],
    "interpretation": "nuanced_preference_requires_context"
}
```

---

## Phase 4: Decay & Maintenance (Monat 5-6)

### Einfaches Decay-Modell

```python
class DecayManager:
    """Energie-basiertes Vergessen"""
    
    DECAY_HALF_LIFE_DAYS = 30  # Energie halbiert sich alle 30 Tage
    
    def apply_decay(self, episode: Episode) -> float:
        """Berechne aktuelle Energie"""
        days_since_access = (datetime.now() - episode.last_accessed).days
        
        # Exponentielles Decay
        decay_factor = 0.5 ** (days_since_access / self.DECAY_HALF_LIFE_DAYS)
        
        # Frequenz-Boost
        frequency_boost = min(2.0, 1 + log(1 + episode.access_count))
        
        current_energy = episode.saliency * decay_factor * frequency_boost
        
        return current_energy
    
    def prune_low_energy(self, threshold: float = 0.1):
        """Markiere schwache Episoden als 'latent'"""
        for episode in self.memory.l1.all():
            energy = self.apply_decay(episode)
            
            if energy < threshold:
                episode.status = "latent"  # Nicht löschen, nur unsichtbar machen
```

### Inkrementelle Konsolidierung (statt Batch)

```python
class IncrementalConsolidator:
    """Konsolidierung während Idle-Zeit, nicht als großer Batch"""
    
    async def background_consolidation(self):
        """Läuft im Hintergrund, wenn Agent idle ist"""
        
        while True:
            await asyncio.sleep(60)  # Jede Minute prüfen
            
            if self.is_idle():
                # Kleine Portionen verarbeiten
                await self.consolidate_chunk(max_items=10)
    
    async def consolidate_chunk(self, max_items: int):
        """Verarbeite nur wenige Items pro Durchlauf"""
        
        # Hole Kandidaten für L1 → L2
        candidates = self.memory.l1.get_promotion_candidates(limit=max_items)
        
        for candidate in candidates:
            if self.gates.check_L1_to_L2(candidate):
                # Abstraktion erstellen
                pattern = await self.extract_pattern(candidate)
                self.memory.l2.add(pattern)
                
                # Episode bleibt, aber mit niedrigerer Priorität
                candidate.promoted = True
```

**Vorteil:**  
- Keine großen "Schlaf-Phasen"
- Kontinuierliche, ressourcenschonende Verarbeitung
- Nutzer merken nichts

---

## Phase 5: Testing & Validation (Monat 6-12)

### Empirische Validierung

#### Experimente-Design

**Experiment 1: Gedächtnis-Persistenz**
```
Setup: 10 User, 30 Tage, tägliche Interaktion
Metrik: Wie viele Präferenzen bleiben nach 30 Tagen abrufbar?
Ziel: >80% Retention für häufig erwähnte Präferenzen
```

**Experiment 2: TLTM/HLTM Trennung**
```
Setup: Gemischte Inhalte (Code + persönliche Präferenzen)
Metrik: Fehlerrate bei Kategorisierung
Ziel: <10% Fehlzuordnung
```

**Experiment 3: Meta-Wissen**
```
Setup: Frage zu unbekanntem Thema
Metrik: Sagt der Agent "Ich weiß es nicht"?
Ziel: >70% korrekte Unsicherheits-Signale
```

#### Quantitative Metriken

```python
class PerformanceMetrics:
    """Messbare Erfolgs-Indikatoren"""
    
    def calculate_maturity_score(self, agent) -> dict:
        return {
            # Strukturelle Reife
            'abstraction_ratio': len(agent.l3) / max(len(agent.l1), 1),
            'consolidation_rate': self.successful_promotions / self.total_candidates,
            
            # Genauigkeit
            'retrieval_precision': self.relevant_retrieved / self.total_retrieved,
            'hallucination_rate': self.false_claims / self.total_claims,
            
            # Effizienz
            'response_time_p95': self.percentile(self.response_times, 0.95),
            'memory_footprint_mb': self.get_db_size_mb(),
            
            # Meta-Kognition
            'uncertainty_accuracy': self.correct_uncertainty_signals / self.total_uncertain_cases
        }
```

### User-Testing-Protokoll

```markdown
## Woche 1-4: Alpha-Test (5 Power-User)
- Täglich 10+ Interaktionen
- Explizites Feedback zu Gedächtnis-Fehlern
- Wöchentliche Interviews

## Woche 5-12: Beta-Test (50 User)
- Normale Nutzung
- Implizite Metriken (Satisfaction, Task Success)
- Monatliche Surveys

## Woche 13+: Public Preview
- Opt-in für Community
- Anonymisierte Telemetrie
- Issue-Tracking auf GitHub
```

---

## Technische Specs für OpenClaw Integration

### API-Design

```python
class AgentMemoryAPI:
    """Public API für OpenClaw-Integration"""
    
    def remember(self, content: str, domain: str = "auto", 
                 importance: float = 1.0) -> str:
        """Speichere Information"""
        episode_id = self.memory.store(
            content=content,
            domain=self._detect_domain(domain, content),
            saliency=importance
        )
        return episode_id
    
    def recall(self, query: str, context: dict = None) -> List[dict]:
        """Rufe relevante Erinnerungen ab"""
        results = self.memory.search(query, context=context)
        
        # Mit Meta-Information anreichern
        return [{
            'content': r.content,
            'confidence': r.confidence,
            'age_days': (datetime.now() - r.timestamp).days,
            'source_layer': r.layer  # L1, L2, oder L3
        } for r in results]
    
    def assess_knowledge(self, topic: str) -> dict:
        """Meta-Wissen abfragen"""
        return self.insight.assess_knowledge(topic)
    
    def consolidate_now(self):
        """Manuelle Konsolidierung triggern (für Tests)"""
        asyncio.create_task(self.consolidator.consolidate_chunk(max_items=100))
```

### Konfigurations-File

```yaml
# memory_config.yaml
memory:
  layers:
    l1:
      retention_days: 7
      max_entries: 10000
    l2:
      retention_days: 90
      max_entries: 5000
    l3:
      retention_days: null  # permanent
      max_entries: 1000
  
  promotion_gates:
    l1_to_l2:
      frequency_min: 2
      recency_days: 7
    l2_to_l3:
      frequency_min: 5
      confidence_min: 0.7
  
  decay:
    half_life_days: 30
    prune_threshold: 0.1
    
  domains:
    tltm:
      conflict_resolution: "override_old"
    hltm:
      conflict_resolution: "hold_variants"
```

---

## Risiken & Mitigations (realistisch)

### Risiko 1: Zu komplex für v1
**Symptom:** Entwicklung dauert >6 Monate  
**Mitigation:** Feature-Freeze nach Monat 4, Rest in v2

### Risiko 2: Schlechte Kategorisierung (TLTM/HLTM)
**Symptom:** Häufige Fehlzuordnung von Inhalten  
**Mitigation:** Manuelle Override-Option für User, Logging für Verbesserung

### Risiko 3: Zu langsam
**Symptom:** Antwortzeit >500ms  
**Mitigation:** Caching, Index-Optimierung, Vector-DB-Tuning

### Risiko 4: User verstehen Konzept nicht
**Symptom:** "Warum vergisst der Agent?"  
**Mitigation:** Transparente UI ("Der Agent erinnert sich an X, lernt noch über Y")

---

## Success Criteria (minimal)

Nach 6 Monaten sollte der Agent:

✅ **Grundfunktionen:**
- [ ] Speichert Episoden in L1
- [ ] Promoted häufige Muster zu L2/L3
- [ ] Wendet Decay auf alte Episoden an
- [ ] Trennt TLTM/HLTM korrekt in >70% der Fälle

✅ **Meta-Kognition:**
- [ ] Kann Coverage zu Thema einschätzen
- [ ] Signalisiert Unsicherheit bei <60% Confidence
- [ ] Erklärt Quelle von Wissen ("Du hast mir das 3x gesagt")

✅ **Performance:**
- [ ] Antwortzeit <300ms (95th percentile)
- [ ] Speicher-Footprint <500MB
- [ ] Läuft auf Laptop ohne GPU

✅ **User-Zufriedenheit:**
- [ ] >70% der Tester sagen "Agent erinnert sich besser als vorher"
- [ ] <20% berichten kritische Gedächtnis-Fehler

---

## Nächster Schritt: MVP in 4 Wochen

### Woche 1: Setup
- SQLite-Schema
- Basis-Klassen (Episode, Association, ConsolidatedKnowledge)
- Embedding-Integration (lokal via sentence-transformers)

### Woche 2: Core-Logik
- Promotion Gates
- Einfaches Decay
- TLTM/HLTM Router

### Woche 3: Spatial Memory
- Concept-Clustering
- Coverage-Berechnung
- Meta-Insight-API

### Woche 4: Integration & Test
- OpenClaw-API-Integration
- 5 Alpha-Tester
- Bug-Fixes

---

## Schlussfolgerung

Dieser reduzierte Plan:
- ✅ Behält die konzeptionelle Essenz von Agent Sapiens
- ✅ Ist in 6 Monaten implementierbar
- ✅ Läuft auf Consumer-Hardware
- ✅ Ist empirisch testbar
- ✅ Erlaubt iterative Verbesserung

**Der Weg zum "Sapiens" ist eine Reise, kein Ziel.**  
Dieser Plan macht den ersten Schritt realistisch.