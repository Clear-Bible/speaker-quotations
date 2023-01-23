# Sources

The sources used in creation/provision of this data are listed below. They are all either CC-BY-4.0 or CC-BY-SA-4.0 at time of use.

## FCBH Quotation Data

* **CharacterDetail.txt**: https://github.com/sillsdev/Glyssen/blob/master/GlyssenCharacters/Resources/CharacterDetail.txt
* **CharacterVerse.txt**: https://github.com/sillsdev/Glyssen/blob/master/GlyssenCharacters/Resources/CharacterVerse.txt

## Copenhagen Alliance Versification

* **eng.json**: https://github.com/Copenhagen-Alliance/versification-specification/blob/master/versification-mappings/standard-mappings/eng.json
* **org.json**: https://github.com/Copenhagen-Alliance/versification-specification/blob/master/versification-mappings/standard-mappings/org.json

These files only provide the number of verses in a chapter; they do not provide the ability to get to previous/next verse from the current verse. For that, it is probably best to extract data from the Macula trees.

## Clear Bible Data

* **MACULA-Greek**: https://github.com/Clear-Bible/macula-greek
* **MACULA-Hebrew**: https://github.com/Clear-Bible/macula-hebrew

The MACULA trees have all sorts of goodness on them, for purposes of this project they're at present needed for previous/next verse navigation.

**Update**: I'm actually extracting verse order from Clear translation alignment data, not from the macula trees. The `CharacterVerse.txt` data uses an "English" versification, and using the Greek and Hebrew versification from MACULA trees was not directly applicable for the previous/next verse problem.

## OpenText.org

* **Clause Projection**: https://github.com/OpenText-org/GNT_annotation_v1.0 

Note the clauses with `projected="yes"` attributes.

* **Context Annotation**: https://github.com/OpenText-org/context-annotation/releases/tag/v1.0.0

OpenText 2.0 context annotation data that expands upon the projection information in v1.0. Projected discourse is indicated where a discourse turn with moves contains a discourse turn with moves. Each new level of `<c unit="turn">` indicated another level of depth. 
