# holbertonschool-hbnb
## HBnB - System Architecture Documentation

## High-Level Package Diagram

```mermaid
classDiagram

class PresentationLayer {
    +APIEndpoints
    +Services
}

class Facade {
    +createUser()
    +createPlace()
    +createReview()
    +getPlaces()
}

class BusinessLogicLayer {
    +User
    +Place
    +Review
    +Amenity
}

class PersistenceLayer {
    +UserRepository
    +PlaceRepository
    +ReviewRepository
    +AmenityRepository
}

PresentationLayer --> Facade : uses
Facade --> BusinessLogicLayer : orchestrates
BusinessLogicLayer --> PersistenceLayer : CRUD operations
```

### Explanation

Presentation Layer handles user interaction and API endpoints.
Business Logic Layer contains core models and business rules.
Persistence Layer manages database operations.
Facade Pattern provides a unified interface between Presentation and Business Logic layers.

---

## Business Logic Layer - Class Diagram

```mermaid
classDiagram

class BaseModel {
    +UUID id
    +datetime created_at
    +datetime updated_at
    +save()
    +delete()
}

class User {
    +string name
    +string email
    +string password
    +register()
    +login()
}

class Place {
    +string title
    +string description
    +float price
    +create()
    +update()
}

class Review {
    +string text
    +int rating
    +submit()
}

class Amenity {
    +string name
    +add()
}

BaseModel <|-- User
BaseModel <|-- Place
BaseModel <|-- Review
BaseModel <|-- Amenity

User "1" --> "*" Place : owns
Place "1" --> "*" Review : has
User "1" --> "*" Review : writes
Place "*" --> "*" Amenity : includes
```

### Explanation

Each entity includes a unique identifier and timestamps via BaseModel.
User represents system users.
Place represents listings.
Review represents user feedback.
Amenity represents features of places.
Relationships define ownership, reviews, and associations between entities.

---

## Sequence Diagrams

### User Registration

```mermaid
sequenceDiagram
participant User
participant API
participant Facade
participant UserModel
participant DB

User->>API: POST /register
API->>Facade: createUser(data)
Facade->>UserModel: validate & create
UserModel->>DB: save user
DB-->>UserModel: success
UserModel-->>Facade: user created
Facade-->>API: response
API-->>User: success
```

---

### Place Creation

```mermaid
sequenceDiagram
participant User
participant API
participant Facade
participant PlaceModel
participant DB

User->>API: POST /places
API->>Facade: createPlace(data)
Facade->>PlaceModel: validate & create
PlaceModel->>DB: save place
DB-->>PlaceModel: success
PlaceModel-->>Facade: place created
Facade-->>API: response
API-->>User: success
```

---

### Review Submission

```mermaid
sequenceDiagram
participant User
participant API
participant Facade
participant ReviewModel
participant DB

User->>API: POST /reviews
API->>Facade: createReview(data)
Facade->>ReviewModel: validate
ReviewModel->>DB: save review
DB-->>ReviewModel: success
ReviewModel-->>Facade: review created
Facade-->>API: response
API-->>User: success
```

---

### Fetch Places

```mermaid
sequenceDiagram
participant User
participant API
participant Facade
participant PlaceModel
participant DB

User->>API: GET /places
API->>Facade: getPlaces(filters)
Facade->>PlaceModel: fetch data
PlaceModel->>DB: query places
DB-->>PlaceModel: data
PlaceModel-->>Facade: places list
Facade-->>API: response
API-->>User: places data
```
