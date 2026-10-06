# Summary

The KIParla Forest treebank is a treebank of spoken Italian based on the [KIParla Corpus](https://kiparla.it/)

# Content

The treebank (release 2.19) contains the conversations:

* BOD2018: semistructured interview from the [KIP](https://github.com/KIParla/KIP) module. Two speakers discuss their homes and living situations. They compare life in Bologna to life in the countryside and smaller towns, and discuss student life compared to a more adult lifestyle.
* BOA3017: free conversation from the [KIP](https://github.com/KIParla/KIP) module. Four friends chat over food. A core thread is one member’s internship, for which he is recording and will have to transcribe the same conversation. Around that, they make casual plans, discuss Easter chocolate eggs, tomorrow’s schedule, and swap gossip.
* BOA1003: office-hours conversation from the [KIP](https://github.com/KIParla/KIP) module. A student meets a professor to sort out the requirements for a course on Italian for academic purposes (L2). They agree on a substitute reading and the format of the oral exam.
* BOA1008: office-hours conversation from the [KIP](https://github.com/KIParla/KIP) module. A student visits a professor’s office to confirm the professor is still available as co-supervisor for their thesis. They discuss the submission deadline around the July graduation session.
* BOA1009: office-hours conversation from the [KIP](https://github.com/KIParla/KIP) module. A student visits a professor to ask her to become co-supervisor for a thesis on child language brokering, specifically focused on CODA children (hearing children of deaf adults) who act as interpreters between Italian and Italian Sign Language.
* TOD1005bis: lecture from the [KIP](https://github.com/KIParla/KIP) module. A professor delivers a university lecture on Arabic dialectology. The lecture covers comparative features of Arabic dialects, the relationship between Arabic and the Semitic language family, and the writing systems used for dialects.
* PBB004: semistructured interview from the [ParlaBO](https://github.com/KIParla/ParlaBO) module. An interviewer talks with two interviewees about the city of Bologna and how it has changed (its walls, porticoes and the San Luca basilica), commuting and daily life, their jobs, music and free-time activities.

# Structure of data

## Documents

Each file contains one conversation and starts with document-level metadata, following the [spoken language guidelines](https://grew.fr/spoken-language-guidelines/workgroups/spoken-data/metadata.html), after `# newdoc`:

* `document_id`: identifier of the conversation (also the file name)
* `genre`: `conversation`, `interview` or `lecture`
* `degree_of_spontaneity`: `unplanned`, `planned` or `elicited`
* `number_of_participants`: `monologic`, `dialogic` or `multi-party`, depending on the number of speakers who speak in the file
* `context`: `public`, `private` or `professional`
* `setting`: `face-to-face`, `telephone`, `broadcast` or `online`
* `channels`: `phonic-auditory`, `gestural-visual` or `graphic-visual`
* `symmetry`: `symmetric` or `asymmetric`, from the relationship between participants

## Sentences

Sentences are built starting from the original KIParla segmentation: the corpus is originally transcribed into Transcription Units (TUs), which are meant to be operative concepts, approximately equivalent to intonation units.
In the corpus, TUs are numbered within each conversation starting from 0.

Every sent_id is built from:

* the sentence’s conversation id
* an underscore _
* one or more TU labels joined by underscores

As sentence boundaries can sometimes occurr within a TU, in this case TU ids are suffixed with a, then b, then c, … to show continuation of that same TU across multiple syntactic units.

All sentences also have as metadata:

* `conversation_id`: identifier of the conversation
* `text_conversationanalysis`: original transcription, following conventions described in [description of Jeffersonian notation](https://github.com/KIParla/KIP/blob/main/jefferson-notation.md). If a syntactic unit results from the joining of multiple TUs, these are separated by a pipe (`|`) in the `text_conversationanalysis` field
* `speaker_id`: identifier of the speaker that uttered the units

The `text` field is rebuilt from the token forms and the `SpaceAfter=No` attribute in MISC; multiword tokens appear in their surface form.

Note that not all transcription units were included in the treebank

## Tokens

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
* `Interrupted=Yes` is used for tokens whose uttering is unfinished. These are also marked by a `~` in the form. They are annotated as UPOS `X`, their lemma is their form and they have no morphological features; the original UPOS and lemma are kept in `ExtUPOS` and `ExtLemma` (the latter only when the lemma differs from the form).
* `Lang` is used for forms in a language or variety other than standard Italian. Its value is the language code when the language is identified, `dia` for dialect, `NO_ISO_CODE` when the language was marked by the transcribers but not identified. These forms are analyzed as Italian (including their morphological features); `Foreign=Yes` is not used.
* `Italian` gives the Italian equivalent of a dialectal form, when it is available.
* `Variety` comes from the transcription of variation: `Unsure` appears on all tokens of a transcription unit in which the transcribers marked that another variety is present without saying on which words; `Unassignable` marks forms whose variety could not be assigned (probably Italian). The specific forms in another language or variety are marked by `Lang`.
* `Nonce=Yes` marks nonce or non-standard forms.
* `OverlappingGroup` is valorized with a list of ids, zero-based, that indicate the progressive number of the overlapping group within the TU.

### Cross-sentence references (interactional relations)

* `Backchannel` appears on tokens that function (along with their dependents) as backchannel. It assumes the value of a specific token id (`[sent_id]::[tok_id]`) which is the token that the backchannel is targeting.

* `Coconstruct` appears on tokens that attach with a specific syntactic relations to other tokens in the treebank. The value is composed by the syntactic relation, followed by double colons (`::`), followed by the token identifier (again in `[sent_id]::[tok_id]` format) that should act as head for the current token.

## Morphology

Lemmas and UPOS are manually annotated. Morphological features are assigned automatically by looking up form, lemma and UPOS in a morphological lexicon built from Morph-it! and from the Italian UD treebanks (ISDT, ParlaMint, PoSTWITA); when the lexicon allows more than one analysis, the choice is made with rules based on the syntactic context. Tokens with UPOS `PUNCT`, `INTJ` or `X`, and interrupted words, have no features. `Foreign=Yes` is not used. Following the other Italian treebanks, the lemma of all articles is `il` and the lemma of clitic pronouns is their form (`l'` is `lo`).

## Relations

Besides the Italian relations already documented in UD, the treebank uses the following language-specific subtypes, which are documented in the UD guidelines for Italian:

* [`conj:reform`](https://universaldependencies.org/it/dep/conj-reform.html): reformulation of an expression
* [`discourse:filler`](https://universaldependencies.org/it/dep/discourse-filler.html): filled pauses and function words used as fillers
* [`discourse:tag`](https://universaldependencies.org/it/dep/discourse-tag.html): tags that ask the interlocutor for confirmation
* [`parataxis:insert`](https://universaldependencies.org/it/dep/parataxis-insert.html): inserted reporting or comment clauses (e.g. *dicono*, *sai*)
* [`parataxis:parenth`](https://universaldependencies.org/it/dep/parataxis-parenth.html): parenthetical clauses
* [`parataxis:restart`](https://universaldependencies.org/it/dep/parataxis-restart.html): restart after an abandoned construction

## Metadata

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

# How to contribute

Data is developed in the `not-to-release` folder, where we keep a file for each conversation. These are then converted into the train/dev/test split through command line, e.g. `cat BOD2018.conllu BOA3017.conllu > ../it_kiparlaforest-ud-test.conllu`

You are welcome to contribute through issues or pull requests. In both cases, please do so by linking the files present in the `not-to-release` folder.

Should you find mistakes or inconsistencies pertaining to the original KIParla data, please open an issue or submit a pull request on the appropriate [KIParla repository](https://github.com/KIParla/).

# References

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

# Acknowledgment

This work was supported by COST Action CA21167 —Universality, diversity and idiosyncrasy in language technology ([UniDive](https://unidive.lisn.upsaclay.fr/)).

# Changelog

* 2026-11-15 v2.19
  * Add conversation PBB004 (ParlaBO module)
  * Morphological features re-assigned from a morphological lexicon
  * Interrupted words are annotated as `X` with the form as lemma; the original values are kept in `ExtUPOS` and `ExtLemma`
  * Lemmas of articles (`il`) and clitic pronouns (form) aligned to the other Italian treebanks
  * Language and variation in MISC: `Language` renamed `Lang`, `Variation=Yes` replaced by `Lang`, `Variety` and `Nonce`
  * New language-specific relations documented in UD: `conj:reform`, `discourse:filler`, `discourse:tag`, `parataxis:parenth`, `parataxis:restart` (and extended `parataxis:insert`)
  * Metadata updated with the new conversation and its speakers
  * Each file starts with `# newdoc`, `# document_id` and document-level metadata (genre and description of the speech event)
* 2025-04-30 v2.18
  * Add conversations BOA1003 and BOA1008
  * Better handling of metadata and Coconstruct/Backchannels field in MISC
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
