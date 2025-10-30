# Summary

The KIParla Forest treebank is a treebank of spoken Italian based on the [KIParla Corpus](https://kiparla.it/)

## Content

The treebank (release 2.17) contains the conversations:

* BOD2018: semistructured interview from the [KIP](https://github.com/KIParla/KIP) module. Two speakers discuss their homes and living situations. They compare life in Bologna to life in the countryside and smaller towns, and discuss student life compared to a more adult lifestyle.
* BOA3017: free conversation from the [KIP](https://github.com/KIParla/KIP) module.
Four friends chat over food. A core thread is one member’s internship, for which he is recording and will have to transcribe the same conversation. Around that, they make casual plans, discuss Easter chocolate eggs, tomorrow’s schedule, and swap gossip.

## Structure of data

### Sentences

Sentences are built starting from the original KIParla segmentation: the corpus is originally transcribed into Transcription Units (TUs), which are meant to be operative concepts, approximately equivalent to intonation units.
In the corpus, TUs are numbered within each conversation starting from 0.

Every sent_id is built from:

* the sentence’s conversation id
* an underscore _
* one or more TU labels joined by underscores

As sentence boundaries can sometimes occurr within a TU, in this case TU ids are suffixed with a, then b, then c, … to show continuation of that same TU across multiple syntactic units.

All sentences also have as metadata:

* `conversation_id`: identifier of the conversation
* `jefferson_text`: original transcription, following conventions described in [description of Jeffersonian notation](https://github.com/KIParla/KIP/blob/main/jefferson-notation.md). If a syntactic unit results from the joining of multiple TUs, these are separated by a pipe (`|`) in the `jefferson_text` field
* `speaker_id`: identifier of the speaker that uttered the units

Note that not all transcription units were included in the treebank

### Tokens

Each token is identified by the `KID` (i.e., *KIParla ID*) attribute in MISC, which is unique in each conversation and links to the corresponding pseudo-tokenized `.tsv` file stored in the appropriate [KIParla repository](https://github.com/KIParla/).
Note that not all original tokens have been included in the treebank, in particular *non verbal behaviors* have not been considered syntactic tokens and *short pauses* have been kept as the `PauseAfter=Yes` feature in MISC.

Other attributes that can be found in MISC:

* `Begin`: floating-point seconds from conversation start marking when the transcription unit to which this token belongs begins. It is present on the first syntactic token of a transcription unit (TU).
* `End`: same as `Begin`, but marks the end of transcription unit
* `Intonation` can be `Rising`, `WeaklyRising` or `Falling`
* `Prolonged=Yes` is present when the token is pronounced with any sound prolongation
* `Volume` can assume values `High` or `Low` if the token appears within a portion of speech pronounced with increased/decreased volume of voice
* `PaceFast=Yes` and `PaceSlow=Yes` are used if the token appears within a portion of speech pronounced with increased/decreased pace
* `Truncated=Yes`
* `Unintelligible=Yes` is used for tokens that were marked by transcribers as non intelligible. These are linked by a generic `dep` relation to others tokens in the sentence. Their form is always `x`.
* `Interrupted=Yes` is used for tokens whose uttering is unfinished. These are also marked by a `~` in the form.
* `Variation=Yes` is used for forms that have been syntactically analyzed as Italian in this treebank but show morphosyntactic traits of dialectal variation. This choice will be better refined in future releases when more instances will be available.
* `OverlappingGroup` is valorized with a list of ids, zero-based, that indicate the progressive number of the overlapping group within the TU.

#### Cross-sentence references (interactional relations)

* `Backchannel` appears on tokens that function (along with their dependents) as backchannel. It assumes the value of a specific token id (`[sent_id]-[tok_id]`) which is the token that the backchannel is targeting.

* `Coconstruct` appears on tokens that attach with a specific syntactic relations to other tokens in the treebank. The value is composed by the syntactic relation, followed by double colons (`::`), followed by the token identifier (again in `[sent_id]-[tok_id]` format) that should act as head for the current token.

### Metadata

A `json` file containing metadata for conversations and speakers is provided in the [`not-to-release` folder](./not-to-release/). For full documentation please refer to the [KIP module readme](https://github.com/KIParla/KIP?tab=readme-ov-file#metadata).

For each **conversation**, besides its code, we also provide:

* type
* duration
* number of participants
* code of participants
* relationship between participants
* presence of a moderator
* year of collection
* point of collection

For each **participant**, besides its code, we also provide:

* type
* occupation
* gender
* region of origin
* age range

## How to contribute

Data is developed in the `not-to-release` folder, where we keep a file for each conversation. These are then converted into the train/dev/test split through command line, e.g. `cat BOD2018.conllu BOA3017.conllu > ../test.conllu`

You are welcome to contribute through issues or pull requests. In both cases, please do so by linking the files present in the `not-to-release` folder.

Should you find mistakes or inconsistencies pertaining to the original KIParla data, please open an issue or submit a pull request on the appropriate [KIParla repository](https://github.com/KIParla/).

## References

You are encouraged to cite this paper if you use the KIParla Forest treebank in your work:

> Ludovica Pannitto, Eleonora Zucchini, Silvia Ballarè, Cristina Bosco, Caterina Mauri, and Manuela Sanguinetti. 2025. Introducing KIParla Forest: seeds for a UD annotation of interactional syntax. In _Proceedings of the Eighth International Conference on Dependency Linguistics (Depling, SyntaxFest 2025)_, pages 54–73, Ljubljana, Slovenia. Association for Computational Linguistics.

```
@inproceedings{pannitto-etal-2025-introducing,
    title = "Introducing {KIP}arla Forest: seeds for a {UD} annotation of interactional syntax",
    author = "Pannitto, Ludovica  and
      Zucchini, Eleonora  and
      Ballar{\`e}, Silvia  and
      Bosco, Cristina  and
      Mauri, Caterina  and
      Sanguinetti, Manuela",
    editor = "Haji{\v{c}}ov{\'a}, Eva  and
      Kahane, Sylvain",
    booktitle = "Proceedings of the Eighth International Conference on Dependency Linguistics (Depling, SyntaxFest 2025)",
    month = aug,
    year = "2025",
    address = "Ljubljana, Slovenia",
    publisher = "Association for Computational Linguistics",
    url = "https://aclanthology.org/2025.depling-1.5/",
    pages = "54--73",
    ISBN = "979-8-89176-290-9",
    abstract = "The present project endeavors to enrich the linguistic resources available for Italian by introducing KIParla Forest, a treebank for the KIParla corpus - an existing and well-known resource for spoken Italian. This article contextualizes the project, describes the treebank creation process and design choices, and highlights future plans for next improvements."
}
```

# Changelog

* 2025-11-15 v2.17
  * Initial release in Universal Dependencies.

<pre>
=== Machine-readable metadata (DO NOT REMOVE!) ================================
Data available since: UD v2.17
License: CC BY-NC-SA 4.0
Includes text: yes
Parallel: no
Genre: spoken
Lemmas: manual native
UPOS: manual native
XPOS: not available
Features: automatic with corrections
Relations: manual native
Contributors: Pannitto, Ludovica; Zucchini, Eleonora; Bosco, Cristina; Mauri, Caterina; Sanguinetti, Manuela; Cocco, Esther
Contributing: here source
Contact: ellepannitto@gmail.com
===============================================================================
</pre>
