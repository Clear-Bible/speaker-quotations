# Sources

The sources used in creation/provision of this data are listed below. They are all either CC-BY-4.0 or CC-BY-SA-4.0 at time of use.

## FCBH Quotation Data

* **CharacterDetail.txt**: https://github.com/sillsdev/Glyssen/blob/master/GlyssenCharacters/Resources/CharacterDetail.txt
* **CharacterVerse.txt**: https://github.com/sillsdev/Glyssen/blob/master/GlyssenCharacters/Resources/CharacterVerse.txt

## Copenhagen Alliance Versification

* **eng.json**: https://github.com/Copenhagen-Alliance/versification-specification/blob/master/versification-mappings/standard-mappings/eng.json
* **org.json**: https://github.com/Copenhagen-Alliance/versification-specification/blob/master/versification-mappings/standard-mappings/org.json

## Clear Bible Data

* **MACULA-Greek**: https://github.com/Clear-Bible/macula-greek
* **MACULA-Hebrew**: https://github.com/Clear-Bible/macula-hebrew

The MACULA trees have all sorts of goodness on them, for purposes of this project they're needed for:

* consistent identifiers for OT and NT words
* pronominal referent and subject referent information (used to posit potential speakers from Clear data)

## OpenText.org

* **Context Annotation**: https://github.com/OpenText-org/context-annotation/releases/tag/v1.0.0

OpenText 2.0 context annotation data that expands upon the projection information in v1.0. Projected discourse is indicated where a discourse turn with moves contains a discourse turn with moves. Each new level of `<c unit="turn">` indicated another level of depth. 

Data in the _OpenText_ forms rely on data from the `n1904` analysis.
