# Quotation and Speaker Data

Data in this repository attempts to identify the original language words, in both the Old and New Testaments, translated as quotations (material using "double" and 'single' quotation marks) in various English Bibles. It also attempts to associate speakers with the quotations, where possible, using data from Faith Comes By Hearing (see [Sources.md](sources.md)).

This data is an initial effort, and as with many datasets, it can be sharpened. We plan some improvements, but the data as it currently exists is also useful so it is time to release it for those who might be interested in it.

## Speaker Data

Each _Quotation_ (see **Quotation Data** below) declares a speaker of the quotation using the field `CharacterId` or `SPEAKER`. Information about each of these speakers is available as:

* JSON:  `json\character_detail.semantic_data.json`. 
* TSV: `tsv\character_detail.semantic_data.tsv`.

This information is a reproduction of [FCBH Character Detail](https://github.com/sillsdev/Glyssen/blob/master/GlyssenCharacters/Resources/CharacterDetail.txt) information with extra fields added for SDBH and LN, where appropriate. This information includes:

* **CharacterId** — The identifier of the speaker; this is consistent across OT and NT. This aligns with "SPEAKER" in TSV or JSON that assigns a speaker to a quotation.
* **MaxSpeakers** — The maximum number of speakers that speak when this speaker is activated.
* **Gender** — The gender of the speaker. This primarily indicates whether FCBH intends a male or female voice for the named speaker.
* **Age** — These are ranges, like "child" or "adult". These primarily indicate whether FCBH intends a child, an adult, or possibly an older adult to voice the speaker.
* **Divinity** — This field denotes divinity. Items with `Y` as value are divine. Otherwise the field is empty.
* **Comment** — Unstructured information regarding the speaker, this field is typically blank.
* **FCBHCharacter** — An alternate identifier used by FCBH.
* **SDBH** — A list of possible SDBH (_Semantic Dictionary of Biblical Hebrew_) identifiers associated with the gloss/speaker. This is a superset and does not presently disambiguate between same-named but different speakers (e.g. "Zechariah").
* **LouwNida** — A list of possible Louw-Nida (_Greek-English Lexicon of the New Testament based on Semantic Domains_) identifiers associated with the gloss/speaker. This is a superset and does not presently disambiguate between same-named but different speakers (e.g. "Zechariah").

Please note that we plan future updates to sharpen and expand the data contained in the `SDBH` and `LouwNida` fields. These fields will not always have data as there are many instances of unnamed speakers (e.g., "two other disciples" in John 21:3 and 21:5).

## Quotation Data

Data reflecting the use of quotations in English translations has been processed and created in several steps. Each step may have use, so interim formats have been stored and documented. These include:

* `alignment-quotations-[translation].tsv` — These are tab-delimited files that provide information about quotations (meaning text in double and/or single quotation marks) in a particular translation, but mapped back to the original language (Hebrew, Aramaic, or Greek) based on alignment data available to Clear Bible.
* `SpeakerProjections-[translation].json` — Translation-specific quotation material that is mapped to speaker data available from FCBH. This is an initial attempt to identify speakers for word ranges in the original language text. These are the richest form of data with lists of word objects associated with speaker data. Please note: Just because something is “quoted” in a translation does not mean it should have a speaker assigned and be considered a quotation.
* `[translation]-aligned-projections.tsv` — Tab-delimited files generated from `SpeakerProjections-[translation].json`. Rather than lists of words, these data include start and end with the assumption of contiguous ranges.

Data for the following English translations are available:

* ESV
* NIV84
* NASB95
* NASB77
* NRSV
* CSB
* HCSB
* LEB

New-Testament-Only data representing projections in the OpenText.org analysis are available as well. 

Note also that a "consensus" form of the data, representing largest agreement of quoted material, is available with the translation named "Clear". This does not contain or represent an actual translation; instead it simply models a consensus view of quotations in the Bible when comparing information from the available translations.

### `alignment-quotations-[translation].tsv` 

Files include:

* `/tsv/alignment-quotations-clear.tsv`
* `/tsv/alignment-quotations-csbe.tsv`
* `/tsv/alignment-quotations-esv.tsv`
* `/tsv/alignment-quotations-hcsb.tsv`
* `/tsv/alignment-quotations-leb.tsv`
* `/tsv/alignment-quotations-nasb77.tsv`
* `/tsv/alignment-quotations-nasb95.tsv`
* `/tsv/alignment-quotations-niv84.tsv`
* `/tsv/alignment-quotations-nrsv.tsv`
* There is no OpenText.org data in this format.

These files are generated by a process that evaluates an alignment between an English translation (such as the NIV84) and the original languages which underlie it. The process evaluates the English for single and double quotation marks. It tracks where quotations open and close as well as the _level_ of quotation.

By _level_ (or _depth_), we mean if the text (level 1) contains, say, direct speech (level 2) which contains another citation (level 3). An example of text with this level of quotation is found in Genesis 3:1 (here in the Lexham English Bible [LEB]): 

* _LEVEL 1_ Now the serpent was more crafty than any other wild animal which Yahweh God had made. He said to the woman, 
  * “_LEVEL 2_ Did God indeed say, 
    * ‘_LEVEL 3_ You shall not eat from any tree in the garden’
  * ?” 

Different translations have different editorial practices which lead to different implementations of quoted material, and thus different levels of quotation.

Regarding accuracy of data, the data is only as accurate as the quotation marks encoded in the alignment data that this process analyzes. For example, there are known issues with the quotation marks in the Lexham English Bible (LEB). These shortcomings affect the derived quotation data. 

The tab delimited data is essentially a list of each original language word unit with information about the reference, word, and quotation level. The word stream is interrupted with an indicator of quotation start and stop. The fields are as follows:

* *depth* — The depth of the quotation, as described above. Text defaults at level 1; any level above 1 indicates quoted material of some sort. A level of 0 (zero) indicates that the original language word was not directly aligned with a word in the translation, so its presence or absence in the quotation cannot be confirmed.
* *id* — A numeric identifier taken from the alignment data the extracted quotation material is based on. The numeric identifier includes information on the book number, chapter number, verse number, word number, and word part: `BBCCCVVVWWWP`.
* *clearId* — The identifier to resolve in Clear Bible's MACULA data. This is almost exclusively the same numeric identifier as the `id`, but with `o` ("Old Testament") or `n` ("New Testament") prepended.
* *USFM* — A modified form of a USFM (Universal Standard Format Marker) Bible reference. It includes a Bible book abbreviation (e.g. "GEN") with book, chapter, verse and word numbers. This data is modified with a dash after the word to also include word part.
* *textContent* — The Hebrew, Aramaic, or Greek word at word position in the original language data.

The word stream is broken to delimit start and end of quotations (at level 2) using `+++` and `---`, in manner like the following example from Genesis 3:1–2:

````
1	010030010101	o010030010101	GEN 3:1!10-1	אֱלֹהִ֑ים‎
1	010030010111	o010030010111	GEN 3:1!11-1	וַ‎
1	010030010112	o010030010112	GEN 3:1!11-2	יֹּ֙אמֶר֙‎
1	010030010121	o010030010121	GEN 3:1!12-1	אֶל־‎
1	010030010131	o010030010131	GEN 3:1!13-1	הָ֣‎
1	010030010132	o010030010132	GEN 3:1!13-2	אִשָּׁ֔ה‎
+++ ==> START QUOTE
2	010030010141	o010030010141	GEN 3:1!14-1	אַ֚ף‎
2	010030010151	o010030010151	GEN 3:1!15-1	כִּֽי־‎
2	010030010161	o010030010161	GEN 3:1!16-1	אָמַ֣ר‎
2	010030010171	o010030010171	GEN 3:1!17-1	אֱלֹהִ֔ים‎
3	010030010181	o010030010181	GEN 3:1!18-1	לֹ֣א‎
3	010030010191	o010030010191	GEN 3:1!19-1	תֹֽאכְל֔וּ‎
3	010030010201	o010030010201	GEN 3:1!20-1	מִ‎
3	010030010202	o010030010202	GEN 3:1!20-2	כֹּ֖ל‎
3	010030010211	o010030010211	GEN 3:1!21-1	עֵ֥ץ‎
3	010030010221	o010030010221	GEN 3:1!22-1	הַ‎
3	010030010222	o010030010222	GEN 3:1!22-2	גָּֽן‎
--- ==> END QUOTE
1	010030020011	o010030020011	GEN 3:2!1-1	וַ‎
1	010030020012	o010030020012	GEN 3:2!1-2	תֹּ֥אמֶר‎
1	010030020021	o010030020021	GEN 3:2!2-1	הָֽ‎
````

### `SpeakerProjections-[translation].json`

Files include:

* `/json/SpeakerProjections-clear.json`
* `/json/SpeakerProjections-csbe.json`
* `/json/SpeakerProjections-esv.json`
* `/json/SpeakerProjections-leb.json`
* `/json/SpeakerProjections-nasb77.json`
* `/json/SpeakerProjections-nasb95.json`
* `/json/SpeakerProjections-niv84.json`
* `/json/SpeakerProjections-nrsv.json`
* `/json/SpeakerProjections-OpenText.json` (though the schema differs slightly; uses `GreekWord` instead of `OriginalWord`)


This JSON is the most descriptive form of data representing word-level quotation data mapped to verse-level speaker data available from Faith Comes By Hearing (FCBH). There are two files from FCBH that are relevant.

* [**CharacterDetail.txt**](https://github.com/sillsdev/Glyssen/blob/master/GlyssenCharacters/Resources/CharacterDetail.txt) — Detail information on each unique named character or entity assigned quotation material in the Biblical text. 
* [**CharacterVerse.txt**](https://github.com/sillsdev/Glyssen/blob/master/GlyssenCharacters/Resources/CharacterVerse.txt) — Verse level assignment of named character or entity as speaker.

This JSON provides character level data from both of these files merged together for a reference range assigned to a particular speaker/character/entity. It also provides word-level quotation data (also known as "projections") for quotation ranges within the reference range. This data uses a "speaker key", which is a concatentation of the first verse, last verse, and speaker (e.g. `GEN 1:3|GEN 1:3|God`) which then has a `SpeakerInstance` and `Projections` (which is a list of `ProjectionData`) associated with it.

#### SpeakerInstance object

| Property | Type | Description |
|----------|------|-------------|
| StartVerse | string | USFM start verse of range |
| EndVerse | string | USFM end verse of range |
| Verses | List of strings | List of verses in range, in order |
| CharacterIds | List of strings | List of CharacterIds; first is default |
| Delivery | string | delivery from FCBH data |
| Alias | string | alias from FCBH data |
| QuoteType | string | quote type from FCBH data |
| DefaultCharacterId | string | Default / preferred character Id |
| ParallelPassage | string | not exhaustive or reliable but sometimes useful. From FCBH data |
| Alternates | List of SpeakerInstance | Some areas are ambiguous and the speaker is not certain so alternates are included |

#### ProjectionData object

| Property | Type | Description |
|----------|------|-------------|
| StartWord | string | identifier for word in Hebrew or Greek MACULA |
| EndWord | string | identifier for word in Hebrew or Greek MACULA |
| Words | List of `ProjectionWord` | Specific lists of words in order |
| Depth | integer | Depth of quotation. Initial text level is `1`; incremented for subsequent levels of quotation. |
| StartVerse | string | USFM start verse of projection |
| EndVerse | string | USFM end verse of projection |
| Verses | List of strings | List of verses in range, in order |

#### ProjectionWord object

| Property | Type | Description |
|----------|------|-------------|
| Id | string | identifier for word in Hebrew or Greek MACULA |
| Text | string | Hebrew, Aramaic, or Greek word (or word part) |
| Gloss | string | English equivalent gloss of `Text`, not included in data presently |
| Depth | integer | Depth of quotation. Initial text level is `1`; incremented for subsequent levels of quotation. |

### `[translation]-aligned-projections.tsv`

Files include: 

* `/tsv/Clear-aligned-projections.tsv`
* `/tsv/CSBE-aligned-projections.tsv`
* `/tsv/ESV-aligned-projections.tsv`
* `/tsv/LEB-aligned-projections.tsv`
* `/tsv/NASB77-aligned-projections.tsv`
* `/tsv/NASB95-aligned-projections.tsv`
* `/tsv/NIV84-aligned-projections.tsv`
* `/tsv/NRSV-aligned-projections.tsv`
* `/tsv/OpenText-aligned-projections.tsv` (no alternate speaker data)

Tab-delimited files generated from `SpeakerProjections-[translation].json`. Rather than lists of words, these data include start and end with the assumption of contiguous ranges. The data consists of the following fields:

* KEY
* START VS
* END VS
* SPEAKER
* ALT SPEAKER
* QUOTE TYPE
* QUOTE DELIVERY
* PROJECTION START
* PROJECTION END
* CLEAR START
* CLEAR END

Note that the `KEY` may be duplicated if a range assignment consists of multiple projections but one speaker. Below is an example of this (GEN 18:13) as well as inclusion of alternate speaker information (GEN 18:9).

````
GEN 18:3|GEN 18:5|Abraham (Abram)	GEN 18:3	GEN 18:5	Abraham (Abram)		Normal		GEN 18:3!2-1	GEN 18:5!13-2	o010180030021	o010180050132
GEN 18:3|GEN 18:5|Abraham (Abram)	GEN 18:3	GEN 18:5	Abraham (Abram)		Normal		GEN 18:5!15-1	GEN 18:5!18-1	o010180050151	o010180050181
GEN 18:6|GEN 18:6|Abraham (Abram)	GEN 18:6	GEN 18:6	Abraham (Abram)		Normal		GEN 18:6!7-1	GEN 18:6!14-1	o010180060071	o010180060141
GEN 18:9|GEN 18:9|Abraham (Abram)	GEN 18:9	GEN 18:9	Abraham (Abram)		Normal		GEN 18:9!3-1	GEN 18:9!5-2	o010180090031	o010180090052
GEN 18:9|GEN 18:10|angels at Sodom, two	GEN 18:9	GEN 18:10	angels at Sodom, two	God	Normal		GEN 18:9!7-1	GEN 18:9!8-3	o010180090071	o010180090083
GEN 18:9|GEN 18:10|angels at Sodom, two	GEN 18:9	GEN 18:10	angels at Sodom, two	God	Normal		GEN 18:10!2-1	GEN 18:10!10-2	o010180100021	o010180100102
GEN 18:12|GEN 18:12|Sarah (Sarai) (old)	GEN 18:12	GEN 18:12	Sarah (Sarai) (old)		Normal		GEN 18:12!5-1	GEN 18:12!11-1	o010180120051	o010180120111
GEN 18:13|GEN 18:15|God	GEN 18:13	GEN 18:15	God		Normal		GEN 18:13!5-1	GEN 18:14!10-1	o010180130051	o010180140101
GEN 18:13|GEN 18:15|God	GEN 18:13	GEN 18:15	God		Normal		GEN 18:15!4-1	GEN 18:15!5-1	o010180150041	o010180150051
GEN 18:13|GEN 18:15|God	GEN 18:13	GEN 18:15	God		Normal		GEN 18:15!9-1	GEN 18:15!11-1	o010180150091	o010180150111


