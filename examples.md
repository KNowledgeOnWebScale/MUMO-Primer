
## Examples

A small test dataset can be found [here](./data.jsonld).

### Query the latest measurement for all sensors

```sparql
PREFIX mumo: <http://mumo.be/ns#>

SELECT ?sensor ?max ?value
WHERE {
   
  {
    SELECT 
      ?sensor (max(?time) as ?max)
    WHERE {
    ?obj a <http://def.isotc211.org/iso19156/2011/Observation#OM_Observation>;
      <https://data.vlaanderen.be/ns/observaties-en-metingen#Observatie.uitgevoerdDoor> ?sensor;
      <http://def.isotc211.org/iso19156/2011/Observation#OM_Observation.resultTime> ?time .
    } GROUP BY ?sensor 
  }

  ?obj a <http://def.isotc211.org/iso19156/2011/Observation#OM_Observation>;
       <http://def.isotc211.org/iso19156/2011/Observation#OM_Observation.result> [
         mumo:value ?value
      ];
     <https://data.vlaanderen.be/ns/observaties-en-metingen#Observatie.uitgevoerdDoor> ?sensor;
     <http://def.isotc211.org/iso19156/2011/Observation#OM_Observation.resultTime> ?max.
}
```

### Query all measurements for one sensor (LuchtvochtigheidsSensor)

```sparql
PREFIX mumo: <http://mumo.be/ns#>
BASE <http://mumo.be/ns#>

SELECT ?measurement ?value ?time
WHERE {
   
  ?measurement a <http://def.isotc211.org/iso19156/2011/Observation#OM_Observation>;
       <http://def.isotc211.org/iso19156/2011/Observation#OM_Observation.result> [
         mumo:value ?value
      ];
     <https://data.vlaanderen.be/ns/observaties-en-metingen#Observatie.uitgevoerdDoor> <LuchtvochtigheidsSensor>;
     <http://def.isotc211.org/iso19156/2011/Observation#OM_Observation.resultTime> ?time.
}
```

### For some measurement type (Luchdruk), list all sensors

```sparql
PREFIX mumo: <http://mumo.be/ns#>
BASE <http://mumo.be/ns#>

SELECT distinct ?sensor
WHERE {
   
  ?obj a <http://def.isotc211.org/iso19156/2011/Observation#OM_Observation>;
       <http://def.isotc211.org/iso19156/2011/Observation#OM_Observation.result> [
         a <LuchtdrukMeting>
      ];
     <https://data.vlaanderen.be/ns/observaties-en-metingen#Observatie.uitgevoerdDoor> ?sensor.
}

```


### Query the latest measurement for all sensors that measure heat


### Something something, query measurement after normalization

```sparql
PREFIX mumo: <http://mumo.be/ns#>
BASE <http://mumo.be/ns#>

SELECT ?raw ?rawValue ?calibrated ?calibratedValue
WHERE {
  ?raw a <http://def.isotc211.org/iso19156/2011/Observation#OM_Observation>;
       <http://def.isotc211.org/iso19156/2011/Observation#OM_Observation.result> [
         mumo:value ?rawValue;
       ].
   
  ?calibrated a <http://def.isotc211.org/iso19156/2011/Observation#OM_Observation>;
    <http://def.isotc211.org/iso19156/2011/Observation#ObservationContext.relatedObservation> ?raw;
       <http://def.isotc211.org/iso19156/2011/Observation#OM_Observation.result> [
         mumo:value ?calibratedValue;
       ].
}
```

