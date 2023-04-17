## MuMo Datamodel

Based on OSLO data model ([Sensoren en Bemonstering (Applicatieprofiel)](https://data.vlaanderen.be/doc/applicatieprofiel/sensoren-en-bemonstering/kandidaatstandaard/2022-04-28/)). 
This data model does not only cover observations but also sampling, in the MuMo project sampling is not used.

### Used classes

Primarily classes defined by the OSLO data model are used.
This data model does, however, not include some domain-specific classes, like the actual kind of measurements.

See the second table that defined some new classes. For example `Observatie.resultaat` has `Meting` as range.


Classes used from the OSLO data model.

- [Bemonstering](https://data.vlaanderen.be/doc/applicatieprofiel/sensoren-en-bemonstering/kandidaatstandaard/2022-04-28/#Bemonstering)
- [BenoemdeWaarde](https://data.vlaanderen.be/doc/applicatieprofiel/sensoren-en-bemonstering/kandidaatstandaard/2022-04-28/#BenoemdeWaarde)
- [Kenmerktype](https://data.vlaanderen.be/doc/applicatieprofiel/sensoren-en-bemonstering/kandidaatstandaard/2022-04-28/#Kenmerktype)
- [Object](https://data.vlaanderen.be/doc/applicatieprofiel/sensoren-en-bemonstering/kandidaatstandaard/2022-04-28/#Object)
- [Observatie](https://data.vlaanderen.be/doc/applicatieprofiel/sensoren-en-bemonstering/kandidaatstandaard/2022-04-28/#Observatie)
- [Observatieprocedure](https://data.vlaanderen.be/doc/applicatieprofiel/sensoren-en-bemonstering/kandidaatstandaard/2022-04-28/#Observatieprocedure)
- [RuimtelijkBemonsteringsobject](https://data.vlaanderen.be/doc/applicatieprofiel/sensoren-en-bemonstering/kandidaatstandaard/2022-04-28/#RuimtelijkBemonsteringsobject)
- [Sensor](https://data.vlaanderen.be/doc/applicatieprofiel/sensoren-en-bemonstering/kandidaatstandaard/2022-04-28/#Sensor)

<!-- List classes -->

Newly added classes.

- Meting
- LuchtdrukMeting
- LuchtvochtigheidsMeting

More kinds of measurements to come
 
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

