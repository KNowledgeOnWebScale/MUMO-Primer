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

The `Bemonstering` is a handling that obtains one or more `Bemonsteringsobject`.
In MuMo this would be placing the device.
The Atmosphere is the object that is gettings sampled (related to `bemonsterdObject`).


The `Bemonsteringsobject` is the device, that placed and activates its sensors.
Each device is equipped with `Sensor`s that measures one quality of its surroundings. 
Each reading is an `Observation`, that is not calibrated. 
To calibrate an `Observation`, we introduce a new `Sensor` the `implementeert` a `ObservatieProcedure` that calibrates the `Observatie` and creates a new `Observatie` of the same type that _is_ calibrated.

</section>


### <abbr title="Linked Data Event Streams">LDES</abbr> interpretation

Publishing data with LDES makes replication and synchronization easy by only creating immutable members.
Multiple members represent the same entity at some point in time.
These instances are called versioned members.

Each time a Sampling is issued, a new `Bemonsteringsobject` is created, but they represent the same device entity. There are linked together with the `identificator` property.

<!-- TOOD: ik ben eigenlijk niet heel zeker welke LDES dingen nog vermeld moeten worden ... -->

