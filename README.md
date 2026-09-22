# 📺 Streaming Ontology

An OWL 2 DL ontology modeling the **streaming media domain**: content (movies, TV series, episodes), platforms, geographic/temporal availability, users, viewing events, ratings and watchlists, with a recommendation logic built on SWRL rules.

Developed as a project for the **Semantic Web** course, reusing **schema.org** and **DBpedia** for interoperability.

---

## 🧱 Namespace

| Prefix | URI |
|---|---|
| `so:` (default) | `http://www.semanticweb.org/streaming-ontology#` |
| `schema:` | `https://schema.org/` |
| `dbo:` / `dbr:` | `http://dbpedia.org/ontology/` \| `http://dbpedia.org/resource/` |
| `swrl:` / `swrla:` / `swrlb:` | standard SWRL vocabularies |

## 📊 At a glance

- 🏷️ **45 classes**
- 🔀 **57 object properties**
- 🔢 **25 data properties**
- 🧍 **159 individuals** (sample/test dataset)
- ⚙️ **10 SWRL rules**

## 🌳 Core structure (TBox)

Everything branches from `DomainEntity`, with the following main branches:

- **`AudiovisualContent`** *(⊑ schema:CreativeWork)* → `Movie`, `TVSeries` → `TVSeason` → `TVEpisode`
- **People** *(⊑ schema:Person / dbo:Person)*: `Actor`, `Director`, `Producer`, `Screenwriter`, `Character`, `User`
- **`ActingRole`**: reified node linking an actor, a character, and a content item
- **`ContentAvailability`**: availability of a content item on a platform, with a time window and country
- **`ViewingEvent`**: a user watching a content item (duration, % completed) → inferred subclasses `CompletedViewingEvent` / `PartialViewingEvent`
- **`UserRating`** *(⊑ schema:Rating)*, **`WatchLaterList`** / **`WatchLaterEntry`**
- **`StreamingPlatform`** / **`ProductionCompany`** *(⊑ schema:Organization)*
- **`Genre`**, also reused via `schema:genre`

## ⚙️ SWRL rules

The rules infer implicit knowledge from viewing events and ratings:

| Rule | What it infers |
|---|---|
| `CompletedViewingEvent` / `PartialViewingEvent` | Classifies a viewing event as completed (≥95%) or partial |
| `HighlyEngagedUser` | A user with ≥3 distinct viewing events |
| `ContentCompleter` | A user with ≥2 completed viewings (≥95%) |
| `PreferredActor` / `PreferredDirector` / `PreferredGenre` | Preferences inferred from 2 viewings (≥80% completion) sharing an actor/director/genre |
| `RecommendActor` / `RecommendGenre` | Suggestion based on 2 content items rated ≥4.0 sharing an actor/genre |
| `RecommendContent` | Suggestion for thematic continuity (same director + genre as a content item completed ≥95%) |

> ⚠️ "Inferred" properties/classes (e.g. `hasPreferredActor`, `suggestedGenre`, `HighlyEngagedUser`) are populated **only** by the reasoner — they should not be asserted manually.

## 🛠️ Toolchain

- **[Protégé 5.6.9](https://protege.stanford.edu/)** with the **SWRLTab** plugin
- **Pellet** reasoner for classification and SWRL rule execution

## 🚀 How to use it

1. Open `streaming-ontology.owl` in Protégé (or load the file with an OWL library such as [Owlready2](https://owlready2.readthedocs.io/) / [Apache Jena](https://jena.apache.org/) for Python/Java)
2. Enable the **Pellet** reasoner (`Reasoner → Pellet → Start reasoner`) to classify inferred classes/properties and execute the SWRL rules
3. Explore the ABox (159 individuals) already included for testing and SPARQL queries

```
Reasoner → Pellet → Start reasoner
SWRLTab   → Execute (to apply the rules)
```
