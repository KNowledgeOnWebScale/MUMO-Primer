## MuMo Datamodel

Based on OSLO data model ([Sensoren en Bemonstering (Applicatieprofiel)](https://data.vlaanderen.be/doc/applicatieprofiel/sensoren-en-bemonstering/kandidaatstandaard/2022-04-28/)). 
This data model does not only cover observations but also sampling, in the MuMo project sampling is not used.

### Used classes

Primarily classes defined by the OSLO data model are used.
This data model does, however, not include some domain-specific classes, like the actual kind of measurements.

See the second table that defined some new classes. For example `Observatie.resultaat` has `Meting` as range.


Classes used from the OSLO data model.

- [Bemonstering](https://data.vlaanderen.be/doc/applicatieprofiel/sensoren-en-bemonstering/kandidaatstandaard/2022-04-28/#Bemonstering) 
  Executing a procedure that will result in `Observatie`s, here mounting a measuring device.
- [BenoemdeWaarde](https://data.vlaanderen.be/doc/applicatieprofiel/sensoren-en-bemonstering/kandidaatstandaard/2022-04-28/#BenoemdeWaarde)
  The object that represents a set parameter for a `ObservatieProcedure`. There are `ObservatieProcedure`s that calibrate measurements, an example `BenoemdeWaarde` is the delta used to calibrate the value.
- [Kenmerktype](https://data.vlaanderen.be/doc/applicatieprofiel/sensoren-en-bemonstering/kandidaatstandaard/2022-04-28/#Kenmerktype)
  The object that represents the real world property that is measured.
- [Object](https://data.vlaanderen.be/doc/applicatieprofiel/sensoren-en-bemonstering/kandidaatstandaard/2022-04-28/#Object)
  The real world object of which properties are measured.
- [Observatie](https://data.vlaanderen.be/doc/applicatieprofiel/sensoren-en-bemonstering/kandidaatstandaard/2022-04-28/#Observatie)
  A measure instance.
- [Observatieprocedure](https://data.vlaanderen.be/doc/applicatieprofiel/sensoren-en-bemonstering/kandidaatstandaard/2022-04-28/#Observatieprocedure)
  The used procedure that resulted in an `Observatie`. Here calibration is such a procedure.
- [RuimtelijkBemonsteringsobject](https://data.vlaanderen.be/doc/applicatieprofiel/sensoren-en-bemonstering/kandidaatstandaard/2022-04-28/#RuimtelijkBemonsteringsobject)
  The real world object that hosts the sensors that measure the object.
- [Sensor](https://data.vlaanderen.be/doc/applicatieprofiel/sensoren-en-bemonstering/kandidaatstandaard/2022-04-28/#Sensor)
  The sensor that measured the object.

<!-- List classes -->

Newly added classes. The base uri for Mumo is `http://mumo.be/ns#` and has a prefered prefix `mumo`.

- `mumo:Meting`
  The abstract class that represents the measurement of an `Observatie`.
- `mumo:LuchtdrukMeting`
  Subclass of `mumo:Meting` representing the measurements of atmospheric pressure.
- `mumo:LuchtvochtigheidsMeting`
  Subclass of `mumo:Meting` representing the measurements of relative humidity.

More kinds of measurements to come

<aside class="note">
In Dutch `Meting` is used both for Measure and Measurement. Here the `Observatie` is the measure and the _Meting_ is the measurement, the result of the measure. 
</aside>
 
<!-- Total visualisation -->
<section data-include="diagram.html"></section>

<section class="informative">

### Explaining the data model with real-world objects

Mumo is often used to measure the atmosphere around museum pieces. 
The atmosphere corresponds with an Object in this data model. 
Each room of a museum has a different atmosphere, because, for example, the temperature is different. 

The atmosphere is modeled in JSON-LD like this.
```json
{
  "@context": "https://raw.githubusercontent.com/KNowledgeOnWebScale/MUMO-Primer/main/context.jsonld",
  "@id": "Atmosphere-van-Momu",
  "@type": "Object"
}
```

The purpose of Mumo is to measure this atmosphere, to do this sensors should be mounted on a device and mounted inside the correct room. 
Each sensor of the device measures a specific feature (_kenmerkType_), like temperature.
The device that contains all sensors is called a SamplingFeature (_Bemonsteringsobject_) and hosts multiple sensors. 
Because it is placed and takes up physical space, the type is specified as a RuimtelijkBemonsteringsobject, which has a property geometry.

These concepts are modeled like this in JSON-LD.
```json
{
  "@context": "https://raw.githubusercontent.com/KNowledgeOnWebScale/MUMO-Primer/main/context.jsonld",
  "@graph": [
    {
      "@id": "TemperatureSensor", 
      "@type": "Sensor",
      "Sensor.observeert": {"@id": "Temperature", "@type": "Kenmerktype"}
    },
    {
      "@id": "MUMO-1", 
      "@type": "RuimtelijkBemonsteringsobject", 
      "Bemonstering.bemonsterdObject": {"@id": "Atmosfeer-van-Momu"},
      "RuimtelijkBemonsteringsobject.gehostPlatform": [ {"@id": "TemperatureSensor"} ],
      "geometrie": {
        "wkt": "POINT(51.21724189528136 4.399529156132677)"^^<http://schemas.opengis.net/geosparql/1.0/geosparql_vocab_all.rdf#wktLiteral>
      }
    }
  ]
}
```

Each time a device is mounted this object is created. Mounting the device is modeled with a Sampling (_bemonstering_).

```json
{
  "@context": "https://raw.githubusercontent.com/KNowledgeOnWebScale/MUMO-Primer/main/context.jsonld",
  "@id": "AtmosfeerBemonstering",
  "@type": "Bemonstering", 
  "bemonsteredObject": {"@id": "Atmosfeer"}, 
  "Bemonstering.resultaat": {"@id": "LuchtMeter"}, 
  "Metadata.beschrijving": "Het herophangen van het RuimtelijkBemonsteringsobject" 
}
```

A measurement is called an observation (_Observatie_). It contains a time, a link to the used sensor and a measured value.
This value has a numeric value and the unit of this value. 

In JSON-LD this looks like this.
```json
{
  "@context": "https://raw.githubusercontent.com/KNowledgeOnWebScale/MUMO-Primer/main/context.jsonld",
  "@id": "observatie/temperature/1/raw",
  "@type": "Observatie",
  "Observatie.resultaattijd": "2002-08-13T16:33:18+02:00",
  "Observatie.uitgevoerdDoor": {"@id": "TemperatureSensor"}, 
  "Observatie.resultaat": {
    "@type": "LuchtdrukMeting",
    "Meting.value": "22.3",
    "Meting.eenheid": "celsius",
    "Meting.gekalibreerd": "false"
  }
}
```

Optionally observations can be calibrated. 
This is useful when a sensor is still precise but has low accuracy. 
This calibration applies an offset to the measurement resulting in a measurement that is precise and accurate.
This process is executed by a calibrator, a type of sensor with a specific calibration procedure.
This procedure contains the used offset that makes an observation accurate.

```json
{
  "@context": "https://raw.githubusercontent.com/KNowledgeOnWebScale/MUMO-Primer/main/context.jsonld",
  "@graph": [
    {
      "@id": "TemperatureCalibration-1",
      "@type": "Observatieprocedure", 
      "Observatieprocedure.input": {"Input.type": {"@id": "Observatie"}}, 
      "Observatieprocedure.output": {"Output.type": {"@id": "Observatie"}},
      "Observatieprocedure.parameter": {"@type": "BenoemdeWaarde", "BenoemdeWaarde.naam": "Delta", "BenoemdeWaarde.waarde": "0.5"}},
    },
    {
      "@id": "LuchtdrukKalibrator", 
      "@type": "Sensor", 
      "Sensor.observeert": {"@id": "Temperature", "@type": "Kenmerktype"}, 
      "Sensor.implementeert": {"@id": "TemperatureCalibration-1"}
    }
  ]
}
```

This calibration sensor creates new associated observations, a property associatedObservation (_geassocieerdeObservatie_) maintains provenance.

```json
{
  "@context": "https://raw.githubusercontent.com/KNowledgeOnWebScale/MUMO-Primer/main/context.jsonld",
  "@id": "observatie/temperature/1",
  "@type": "Observatie",
  "Observatie.resultaattijd": "2002-08-14T16:33:23+02:00",
  "Observatie.resultaat": { "@type": "Meting", "Meting.value": "21.8", "Meting.eenheid": "celsius", "Meting.gekalibreerd": "true" },
  "Observatie.uitgevoerdDoor": { "@id": "TemperatureCalibration-1"}, 
  "Observatie.geassocieerdeObservatie": {"@id": "observatie/temperature/1/raw"}
}
```

A full example of this data can be found [here](./data.jsonld).

</section>


### <abbr title="Linked Data Event Streams">LDES</abbr> interpretation

Publishing data with LDES makes replication and synchronization easy by only creating immutable members.
Multiple members represent the same entity at some point in time.
These instances are called versioned members.

Each time a Sampling is issued, a new `Bemonsteringsobject` is created, but they represent the same device entity. There are linked together with the `identificator` property.

<!-- TOOD: ik ben eigenlijk niet heel zeker welke LDES dingen nog vermeld moeten worden ... -->

