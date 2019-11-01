
## Queries

### Alle Lifte holen mit Namen und Status

```
query allLifts {
  allLifts {
    name
    status
  }
}
```


### Queries mit Argumenten

```
query allOpenLifts {
  allLifts(status: OPEN){
    id
    name
  }
}
```

### Queries mit mehreren Root-Abfragen

```
query allLiftsAndTrails {
  allLifts {
    name
    status
  }
  allTrails {
    name
    difficulty
  }
}
```

### Queries mit Naming/Renaming/Aliasse

Benenne ein paar Felder um:

```
query liftsAndTrailsAliase {
  anzahlLifte: liftCount(status: OPEN)
  lifts: allLifts {
    name
    zustand: status
  }
  trails: allTrails {
    name
    schwierigkeit: difficulty
  }
}

```

### Queries mit Aliasse und Labels

Holle alle offenen und geschlossenen Lifte:

```
query openLiftsAndClosedLifts {
  anzahlLifte: liftCount(status: OPEN)
  openLifts: allLifts(status: OPEN) {
    name
    zustand: status
  }
  trails: allTrails {
    name
    schwierigkeit: difficulty
  }
  closedLifts: allLifts(status: CLOSED) {
    name
    zustand: status
  }
}


```


### Queries mit verschachtelten Objekten 1

Hole alle Trails die man über den Lift jazz-cat erreichen kann:

```
query trailsAccessedByJazzCat {
  Lift(id: "jazz-cat") {
    capacity
    trailAccess {
      name
      difficulty
    }
  }
}

```


### Queries mit verschachtelten Objekten 2

Hole alle Trails die erreichbar sind durch den Lift "dance-fight"

```
query liftToAccessTrail {
  Trail(id: "dance-fight") {
    groomed
    accessedByLifts {
      name
      capacity
    }
  }
}

```


### Fragments

Ohne Fragments, nicht DRY:

```

query Fragments {
  Lift(id: "jazz-cat") {
    name
    status
    capacity
    night
    elevationGain
    trailAccess {
      name
      difficulty
    }
  }
  Trail(id: "river-run") {
    name
    difficulty
    accessedByLifts {
      name
      status
      capacity
      night
      elevationGain
    }
  }
}

```

Mit Fragments, ist DRY:

```
query {
  Lift(id: "jazz-cat") {
    ...liftInfo
    trailAccess {
      name
      difficulty
    }
  }
  Trail(id: "river-run") {
    name
    difficulty
    accessedByLifts {
      ...liftInfo
    }
  }
}

fragment liftInfo on Lift {
  name 
  status
  capacity
  night
  elevationGain
}

```


### Mutations

```
query isLiftOpen {
  Lift(id: "jazz-cat") {
    name
    status
  }
}

```

```
mutation closeLift {
  setLiftStatus(id: "jazz-cat", status: CLOSED) {
    name
    status
  }
}

```


### Variablen

```
mutation closeLift($id: ID!, $status:LiftStatus!) {
  setLiftStatus(id: $id, status: $status) {
    name
    status
  }
}
```

```
{
  "id": "jazz-cat",
  "status": "OPEN"
}
```


### Subscriptions

```

subscription {
  liftStatusChange {
    id
    name
    status
  }
}
```


```

mutation closeLift {
  setLiftStatus(id: "jazz-cat", status: CLOSED) {
    name
    status
  }
}

```
