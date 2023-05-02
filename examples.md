
## Examples

A small example dataset can be found [here](./data.jsonld).

### Query the latest measurement for all sensors

```language-sparql
PREFIX Observatie: <http://def.isotc211.org/iso19156/2011/Observation#OM_Observation>
PREFIX uitgevoerdDoor: <https://data.vlaanderen.be/ns/observaties-en-metingen#Observatie.uitgevoerdDoor>
PREFIX resultaat: <http://def.isotc211.org/iso19156/2011/Observation#OM_Observation.result>
PREFIX resultaatTijd: <http://def.isotc211.org/iso19156/2011/Observation#OM_Observation.resultTime>

PREFIX mumo: <http://mumo.be/ns/>


SELECT ?sensor ?max ?value
WHERE {
  {
    SELECT ?sensor (max(?time) as ?max)
    WHERE {
      ?obj a Observatie: ;
        uitgevoerdDoor: ?sensor;
        resultaatTijd: ?time .
    } GROUP BY ?sensor 
  }

  ?obj a Observatie: ;
       resultaat: [
         mumo:value ?value
      ];
     uitgevoerdDoor: ?sensor;
     resultaatTijd: ?max.
}
```

### Query all measurements for one sensor (LuchtvochtigheidsSensor)

```language-sparql
PREFIX Observatie: <http://def.isotc211.org/iso19156/2011/Observation#OM_Observation>
PREFIX uitgevoerdDoor: <https://data.vlaanderen.be/ns/observaties-en-metingen#Observatie.uitgevoerdDoor>
PREFIX resultaat: <http://def.isotc211.org/iso19156/2011/Observation#OM_Observation.result>
PREFIX resultaatTijd: <http://def.isotc211.org/iso19156/2011/Observation#OM_Observation.resultTime>

PREFIX mumo: <http://mumo.be/ns/>
BASE <http://mumo.be/data/>


SELECT ?measurement ?value ?time
WHERE {
  ?measurement a Observatie: ;
     resultaat: [
       mumo:value ?value
     ];
     uitgevoerdDoor: <LuchtvochtigheidsSensor>;
     resultaatTijd: ?time.
}
```

### For some measurement type (Luchdruk), list all sensors

```language-sparql
PREFIX Observatie: <http://def.isotc211.org/iso19156/2011/Observation#OM_Observation>
PREFIX uitgevoerdDoor: <https://data.vlaanderen.be/ns/observaties-en-metingen#Observatie.uitgevoerdDoor>
PREFIX resultaat: <http://def.isotc211.org/iso19156/2011/Observation#OM_Observation.result>

BASE <http://mumo.be/data/>


SELECT distinct ?sensor
WHERE {
   
  ?obj a Observatie: ;
       resultaat: [
         a <LuchtdrukMeting>
      ];
     uitgevoerdDoor: ?sensor.
}

```


### Query the latest measurement for all sensors that measure heat


### Query all measurements after calibration

```language-sparql
PREFIX Observatie: <http://def.isotc211.org/iso19156/2011/Observation#OM_Observation>
PREFIX uitgevoerdDoor: <https://data.vlaanderen.be/ns/observaties-en-metingen#Observatie.uitgevoerdDoor>
PREFIX resultaat: <http://def.isotc211.org/iso19156/2011/Observation#OM_Observation.result>
PREFIX resultaatTijd: <http://def.isotc211.org/iso19156/2011/Observation#OM_Observation.resultTime>
PREFIX geassocieerdeObservatie: <http://def.isotc211.org/iso19156/2011/Observation#ObservationContext.relatedObservation>

PREFIX mumo: <http://mumo.be/ns/>
BASE <http://mumo.be/data/>


SELECT ?raw ?rawValue ?calibrated ?calibratedValue
WHERE {
  ?raw a Observatie: ;
    resultaat: [
       mumo:value ?rawValue;
    ].
   
  ?calibrated a Observatie: ;
    geassocieerdeObservatie: ?raw;
      resultaat: [
        mumo:value ?calibratedValue;
      ].
}
```

