# Formato JSON

Os dados JSON exportados seguirão o formato descrito abaixo. Todos os dados serão encapsulados em um objeto `data` com arrays para cada tipo de dados suportado conforme indicado abaixo:

```json
"data": {
    "metrics": <Array>,
    "workouts": <Array>,
    "stateOfMind": <Array>,
    "medications": <Array>,
    "symptoms": <Array>,
    "cycleTracking": <Array>,
    "ecg": <Array>,
    "heartRateNotifications": <Array>
}
```

## Métricas de Saúde

O campo `metrics` é um array de objetos, cada um contendo dados para uma métrica de saúde específica. A maioria dos dados de métricas de saúde segue uma estrutura comum. Exceções a este formato também são documentadas abaixo:

### Formato Comum

- `name`: `<String>`
- `units`: `<String>`
- `data`: `<Array>`

O array `data` anexado a uma métrica de saúde conterá um valor de quantidade que corresponde ao valor `units` associado, junto com um timestamp:

```json
"data": [
    {
        "qty": <Number>,
        "date": <Date (yyyy-MM-dd HH:mm:ss Z)>
    }
]
```

### Pressão Arterial

```json
"data": [
    {
        "date": <Date (yyyy-MM-dd HH:mm:ss Z)>,
        "systolic": <Number>,
        "diastolic": <Number>
    }
]
```

### Heart Rate

```json
"data": [
    {
        "date": <Date (yyyy-MM-dd HH:mm:ss Z)>,
        "Min": <Number>,
        "Avg": <Number>,
        "Max": <Number>
    }
]
```

### Análise do Sono

Os payloads de dados de sono podem variar dependendo se a agregação de dados está ativada ou desativada.

#### Agregado

```json
"data": [
    {
        "date": <Date (yyyy-MM-dd)>,
        "totalSleep": <Number>,
        "asleep": <Number>,
        "core": <Number>,
        "deep": <Number>,
        "rem": <Number>,
        "sleepStart": <Date (yyyy-MM-dd HH:mm:ss Z)>,
        "sleepEnd": <Date (yyyy-MM-dd HH:mm:ss Z)>,
        "inBed": <Number>,
        "inBedStart": <Date (yyyy-MM-dd HH:mm:ss Z)>,
        "inBedEnd": <Date (yyyy-MM-dd HH:mm:ss Z)> ,
    }
]
```

#### Não Agregado

```json
"data": [
    {
        "startDate": <Date (yyyy-MM-dd)>,
        "endDate": <Number>,
        "qty": <Number>,
        "value": <String> (Awake | Asleep* | In Bed | Core | REM | Deep | Unspecified),
        "deep": <Number>,
        "rem": <Number>,
        "sleepStart": <Date (yyyy-MM-dd HH:mm:ss Z)>,
        "sleepEnd": <Date (yyyy-MM-dd HH:mm:ss Z)>,

        "inBed": <Number>,
        "inBedStart": <Date (yyyy-MM-dd HH:mm:ss Z)>,
        "inBedEnd": <Date (yyyy-MM-dd HH:mm:ss Z)> ,
    }
]
```

\*`Asleep` refere-se a uma fase de sono não categorizada (em vez do tempo total dormindo). Isso pode ocorrer se a fonte de dados não suportar rastreamento de fases de sono.

### Glicose Sanguínea

```json
"data": [
    {
        "date": <Date (yyyy-MM-dd HH:mm:ss Z)>,
        "qty": <Number>,
        "mealTime": <String> ("Before Meal" | "After Meal" | "Unspecified")
    }
]
```

### Atividade Sexual

```json
"data": [
    {
        "date": <Date (yyyy-MM-dd HH:mm:ss Z)>,
        "Unspecified":<Number>,
        "Protection Used":<Number>,
        "Protection Not Used":<Number>,
    }
]
```

### Lavagem de Mãos

```json
"data": [
    {
        "date": <Date (yyyy-MM-dd HH:mm:ss Z)>,
        "qty": <Number>,
        "value":<String> ("Complete" | "Incomplete")
    }
]
```

### Escovação de Dentes

```json
"data": [
    {
        "date": <Date (yyyy-MM-dd HH:mm:ss Z)>,
        "qty": <Number>,
        "value":<String> ("Complete" | "Incomplete")
    }
]
```

### Administração de Insulina

```json
"data": [
    {
        "date": <Date (yyyy-MM-dd HH:mm:ss Z)>,
        "qty": <Number>,
        "reason":<String> ("Bolus" | "Basal")
    }
]
```

## Medicamentos

```json
{
    "displayText": <String>,
    "nickname": <String | undefined>,
    "start": <Date (yyyy-MM-dd HH:mm:ss Z)>,
    "end": <Date (yyyy-MM-dd HH:mm:ss Z) | undefined>,
    "scheduledDate": <Date (yyyy-MM-dd HH:mm:ss Z) | undefined>,
    "form": <String> ("Capsule" | "Cream" | "Device" | "Drops" | "Foam" | "Gel" | "Inhaler" | "Injection" | "Liquid" | "Lotion" | "Ointment" | "Patch" | "Powder" | "Spray" | "Suppository" | "Tablet" | "Topical" | "Unknown"),
    "status": <String> ("Not Interacted" | "Notification Not Sent" | "Snoozed" | "Taken" | "Skipped" | "Not Logged" | "Unspecified"),
    "isArchived": <Boolean>,
    "dosage": <Number | undefined>
    "codings": [
        {
            "code": <String>,
            "system": <String>,
            "version": <String | undefined>,
        }
    ]
}
```

## Sintomas

O Health Auto Export suporta todos os dados de sintomas de saúde.

```json
[
    {
        "start": <Date (yyyy-MM-dd HH:mm:ss Z)>,
        "end": <Date (yyyy-MM-dd HH:mm:ss Z)>,
        "name": <String>,
        "severity": <String>,
        "userEntered": <Boolean>,
        "source": <String>
    }
]
```

## Estado Mental

O Health Auto Export suporta todos os dados de State of Mind.

```json
{
    "id": String,
    "start": String,
    "end": String,
    "kind": String,
    "labels": Array<String>,
    "associations": Array<String>,
    "valence": Number,
    "valenceClassification": Number,
    "metadata": Object<String:String>
}
```

## Notificações de Heart Rate

Os dados de Notificações de Heart Rate são estruturados de forma bastante diferente de outros dados de métricas de saúde. No nível superior, há informações sobre o início e fim do evento. Um limite de evento é incluído para eventos de heart rate alto e baixo.

Metadados adicionais coletados durante o evento de heart rate são incluídos para heart rate e variação de heart rate.

O campo `threshold` é incluído apenas para notificações de heart rate alto e baixo)

```json
[
    {
        "start": <Date (yyyy-MM-dd HH:mm:ss Z)>,
        "end": <Date (yyyy-MM-dd HH:mm:ss Z)>,
        "threshold": <Number> (For high and low heart rate notifications)
        "heartRate": [
            {
                "hr": <Number>,
                    "units": "bpm",
                    "timestamp": {
                        "start": <Date (yyyy-MM-dd HH:mm:ss Z)>,
                        "end": <Date (yyyy-MM-dd HH:mm:ss Z)>,
                        "interval": {
                            "duration": <Number>,
                            "units": "s" (seconds)
                        }
                    }
            }
        ],
        "heartRateVariation": [
            {
                "hrv": <Number>,
                "units": "ms",
                "timestamp": [
                    "start": <Date (yyyy-MM-dd HH:mm:ss Z)>,
                    "end": <Date (yyyy-MM-dd HH:mm:ss Z)>,
                    "interval": [
                        "duration": <Number>,
                        "units": "s"
                    ]
                ]
            }
        ]
    }
]
```

## ECG

O Health Auto Export suporta exportação de dados ECG no seguinte formato:

```json
[
    {
        "start": <Date (yyyy-MM-dd HH:mm:ss Z)>,
        "end": <Date (yyyy-MM-dd HH:mm:ss Z)>,
        "classification": <String> ("Sinus Rhythm" | "Atrial Fibrillation" | "High Heart Rate" | "Inconclusive Low Heart Rate" | "Inconclusive High Heart Rate" | "Inconclusive" | "Inconclusive Poor Recording" | "Inconclusive" | "Unrecognized" ),
        "severity": <String>,
        "averageHeartRate": <Number>,
        "numberOfVoltageMeasurements": <Number>
        "voltageMeasurements": Array<VoltageMeasurement>
        "samplingFrequency": <Number> (Hz)
        "source": <String>
    }
]
```

Voltage Measurement

```json
[
    {
        "date": <Date (yyyy-MM-dd HH:mm:ss Z)>,
        "voltage": <Number>,
        "units": <String>
    }
]
```

## Exercícios

O campo `workouts` é um array de objetos, cada um contendo dados para um exercício específico. Os exercícios seguem uma estrutura comum. As unidades para um campo específico podem ser variáveis, dependendo das preferências de unidades do usuário (ex. "kcal" vs "kJ"), e denotadas como `<String>`, ou as unidades podem ser fixas e expressas por um valor hardcoded.

### Exercícios v2

```json
{
  "id": <String>,
  "name": <String>,
  "start": <Date (yyyy-MM-dd HH:mm:ss Z)>,
  "end": <Date (yyyy-MM-dd HH:mm:ss Z)>,
  "duration": <Number (seconds)>,


  // Optional fields
  "location": <String | undefined>, ("Indoor" | "Outdoor" | "Pool" | "Open Water")
  "activeEnergyBurned": { "qty": <Number>, "units": <String> } | undefined>,
  "intensity": { "qty": <Number>, "units": "MET" } | undefined>,
  "distance": { "qty": <Number>, "units": <String ("mi" | "km")> } | undefined>,
  "temperature": { "qty": <Number>, "units": <String ("degF" | "degC")> } | undefined>,
  "humidity": { "qty": <Number>, "units": "%" } | undefined>,
  "avgSpeed": { "qty": <Number>, "units": <String ("mph" | "kmph")> } | undefined>,
  "maxSpeed": { "qty": <Number>, "units": <String ("mph" | "kmph")> } | undefined>,
  "elevationUp": { "qty": <Number>, "units": <String ("ft" | "m")> } | undefined>,
  "elevationDown": { "qty": <Number>, "units": <String ("ft" | "m")> } | undefined>,
  "lapLength": { "qty": <Number>, "units": <String ("mi" | "km")> } | undefined>,
  "strokeStyle": <String | undefined> ("Backstroke" | "Breaststroke" | "Butterfly" | "Freestyle" | "Mixed" | "Kickboard" | "Uknown"),
  "swolfScore": <Number | undefined>>,
  "salinity": <String | undefined> ("Fresh Water" | "Salt Water"),
  "activeEnergy": <Array(QuntityData) | undefined>,
  "basalEnergy": <Array(QuntityData) | undefined>,
  "cyclingCadence": <Array(QuntityData) | undefined>,
  "cyclingDistance": <Array(QuntityData) | undefined>,
  "cyclingPower": <Array(QuntityData) | undefined>,
  "cyclingSpeed": <Array(QuntityData) | undefined>,
  "swimDistance": <Array(QuntityData) | undefined>,
  "swimStroke": <Array(QuntityData) | undefined>,
  "stepCount": <Array(QuntityData) | undefined>,
  "walkingAndRunningDistance": <Array(QuntityData) | undefined>,
  "heartRateData": <Array(HeartRateData) | undefined>,
  "heartRateRecovery": <Array(HeartRateData) | undefined>,
  "metadata": <String:Any | undefined>,
  "route": <Array(Location) | undefined>
}
```

**QuntityData**

```json
{
    "date": <String (yyyy-MM-dd HH:mm:ss Z)>,
    "qty": <Number>,
    "units": <Stirng>,
    "source": <String>
}
```

**Heart Rate Data**

```json
{
    "date": <String (yyyy-MM-dd HH:mm:ss Z)>,
    "Min": <Number>,
    "Avg": <Number>,
    "Max": <Number>,
    "units": <String>,
}
```

**Location**

```json
{
    "latitude": <Number>,
    "longitude": <Number>,
    "altitude": <Number>,
    "course": <Number>,
    "courseAccuracy": <Number>,
    "horizontalAccuracy": <Number>,
    "verticalAccuracy": <Number>,
    "timestamp": <String>,
    "speed": <Number>,
    "speedAccuracy": <Number>
}
```

### Exercícios v1

```json
[
    {
        "name": <String>,
        "start": <Date (yyyy-MM-dd HH:mm:ss Z)>,
        "end": <Date (yyyy-MM-dd HH:mm:ss Z)>,
        "heartRateData": [
            "date": <Date (yyyy-MM-dd HH:mm:ss Z)>
            "qty": <Number>,
            "units": "count"
        ],
        "heartRateRecovery": [
            {
                "date": <Date (yyyy-MM-dd HH:mm:ss Z)>
                "qty": <Number>,
                "units": "count"
            }
        ],
        "route": [
            {
                "lat": <Number>,
                "lon": <Number>,
                "altitude": <Number (in meters)>,
                "timestamp" <Date (yyyy-MM-dd HH:mm:ss Z)>
            }
        ],
        "totalEnergy": {
            "qty": <Number>,
            "units": <String>
        },
        "activeEnergy": {
            "units": <String>,
            "qty": <Number>
        },
        "maxHeartRate": {
            "qty": <Number>,
            "units": "bpm"
        },
        "avgHeartRate": {
            "qty": <Number>,
            "units": "bpm"
        },
        "stepCount": {
            "qty": <Number>,
            "units": "steps"
        },
        "stepCadence": {
            "qty": <Number>,
            "units": "spm"
        },
        "totalSwimmingStrokeCount": {
            "qty": <Number>,
            "units": "count"
        },
        "swimCadence": {
            "qty": <Number>,
            "units": "spm"
        },
        "distance": {
            "qty": <Number>,
            "units": <String>
        },
        "speed": {
            "qty": <Number>,
            "units": <String>
        },
        "flightsClimbed": {
            "qty": <Number>,
            "units": "count"
        },
        "intesity": {
            "qty": <Number>,
            "units": "MET"
        },
        "temperature": {
            "qty": <Number>,
            "units": <String>
        },
        "humidity": {
            "qty": <Number>,
            "units": "%"
        },
        "elevation": {
            "ascent": <Number>,
            "descent": <Number>,
            "units": <String>
        }
    }
]
```