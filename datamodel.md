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

### Mapping data model to the real world

bemonderstingsding is dit.
meting is dit.
bemonstering is dit.

### LDES interpretation

LDES requirements, versioned objects with an identifier.
This is the case for:

- bemonderstingsding
- andere dingen



