
# Streamy Core Domain Structural Architecture

## Foundational Object-Oriented Entity Specifications

### 1. Conceptual Domain Breakdown

This tracking document establishes the structural domain model for the Streamy streaming platform. It defines the core classes, their attributes, inheritance hierarchy, and relationships to support the platform's object-oriented architecture.

### 2. Advanced Production Structural Class Diagram

The following class diagram models the Streamy domain entities, including inheritance, composition, aggregation, associations, and multiplicity constraints.

```mermaid
---
title: Streamy Production Structural Domain Model
---
classDiagram
    direction TB

    class Title {
        <<abstract>>
        +String name
        +int releaseYear
        +playMedia() Boolean
    }

    class Film {
        +int lengthMinutes
    }

    class TVShow {
        +int seasonsCount
    }

    class Season {
        +int seasonNumber
    }

    class Episode {
        +String episodeTitle
        +int durationSeconds
    }

    class Review {
        +int ratingStars
        +String reviewText
    }

    class Actor {
        +String fullName
    }

    class Genre {
        +String genreName
    }

    %% Inheritance
    Film --|> Title
    TVShow --|> Title

    %% Composition
    TVShow "1" *-- "0..*" Season : owns
    Season "1" *-- "1..*" Episode : contains

    %% Association
    Title "1" -- "0..*" Review : receives

    %% Aggregation
    Title "0..*" o-- "0..*" Actor : features

    %% Genre relationship
    Title "0..*" -- "1..*" Genre : categorized_under
```
