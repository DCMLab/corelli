![Version](https://img.shields.io/github/v/release/DCMLab/corelli?display_name=tag)
[![DOI](https://zenodo.org/badge/343145912.svg)](https://zenodo.org/badge/latestdoi/343145912)
![GitHub repo size](https://img.shields.io/github/repo-size/DCMLab/corelli)
![License](https://img.shields.io/badge/license-CC%20BY--NC--SA%204.0-9cf)


This is a README file for a data repository originating from the [DCML corpus initiative](https://github.com/DCMLab/dcml_corpora)
and serves as welcome page for both 

* the GitHub repo [https://github.com/DCMLab/corelli](https://github.com/DCMLab/corelli) and the corresponding
* documentation page [https://dcmlab.github.io/corelli](https://dcmlab.github.io/corelli)

For information on how to obtain and use the dataset, please refer to [this documentation page](https://dcmlab.github.io/corelli/introduction).


<!-- TOC -->
* [Arcangelo Corelli - Trio Sonatas  (A corpus of annotated scores)](#arcangelo-corelli---trio-sonatas-a-corpus-of-annotated-scores)
  * [Version history](#version-history)
  * [Getting the data](#getting-the-data)
    * [With full version history](#with-full-version-history)
    * [Without full version history](#without-full-version-history)
  * [Data Formats](#data-formats)
    * [Opening Scores](#opening-scores)
    * [Opening TSV files in a spreadsheet](#opening-tsv-files-in-a-spreadsheet)
    * [Loading TSV files in Python](#loading-tsv-files-in-python)
  * [Column names](#column-names)
  * [Generating all TSV files from the scores](#generating-all-tsv-files-from-the-scores)
  * [Score origin](#score-origin)
  * [Caveats](#caveats)
    * [Wrong positions](#wrong-positions)
    * [`.warnings` files](#warnings-files)
    * [Instruments](#instruments)
  * [Questions, Suggestions, Corrections, Bug Reports](#questions-suggestions-corrections-bug-reports)
  * [License](#license)
  * [File naming convention](#file-naming-convention)
  * [Overview](#overview)
<!-- TOC -->

# Arcangelo Corelli - Trio Sonatas  (A corpus of annotated scores)

This corpus of annotated [MuseScore](https://musescore.org) files has been created within
the [DCML corpus initiative](https://github.com/DCMLab/dcml_corpora) and employs
the [DCML harmony annotation standard](https://github.com/DCMLab/standards). It was relased together with and as part 
of the "workflow paper"

> Hentschel, J., Moss, F. C., Neuwirth, M., & Rohrmeier, M. A. (2021). A semi-automated workflow paradigm for the distributed creation and curation of expert annotations. Proceedings of the 22nd International Society for Music Information Retrieval Conference, ISMIR, 262–269. https://doi.org/10.5281/ZENODO.5624417

The corpus comprises 36 `Sonate a tre`, divided into 149 separate movements. Together they make up for
three of the four famous cycles of 12 trio sonatas each:

| Opus | Cycle               | Publication | Included |
|------|---------------------|-------------|----------|
| 1    | 12 sonate da chiesa | Rome 1681   | Yes      |
| 2    | 12 sonate da camera | Rome 1685   | No       |
| 3    | 12 sonate da chiesa | Rome 1689   | Yes      |
| 4    | 12 sonate da camera | Rome 1694   | Yes      |

## Version history

See the [GitHub releases](https://github.com/DCMLab/corelli/releases).


## Getting the data

### With full version history

The dataset is version-controlled via [git](https://git-scm.com/). In order to download the files with all
revisions they have gone through, git needs to be installed on your machine. Then you can clone this 
repository using the command

```bash
git clone https://github.com/DCMLab/corelli.git
```

### Without full version history

If you are only interested in the current version of the corpus, you can simply download and unpack
[this ZIP file](https://github.com/DCMLab/corelli/archive/refs/heads/main.zip).


## Data Formats

Each piece in this corpus is represented by four files with identical names, each in its own folder. For example, 
the first movement of the first sonata has the following files:

* `MS3/op01n01a.mscx`: Uncompressed MuseScore file including the music and annotation labels.
* `notes/op01n01a.tsv`: A table of all note heads contained in the score and their relevant features (not each of them represents an onset, some are tied together)
* `measures/op01n01a.tsv`: A table with relevant information about the measures in the score.
* `harmonies/op01n01a.tsv`: A list of the included harmony labels (including cadences and phrases) with their positions in
  the score.

### Opening Scores

After navigating to your local copy, you can open the scores in the folder `MS3` with the free and open source score
editor [MuseScore](https://musescore.org). Please note that the scores have been edited, annotated and tested with
[MuseScore 3.6.2](https://github.com/musescore/MuseScore/releases/tag/v3.6.2). 
MuseScore 4 has since been released and preliminary tests suggest that it renders them correctly.

### Opening TSV files in a spreadsheet

Tab-separated value (TSV) files are like Comma-separated value (CSV) files and can be opened with most modern text
editors. However, for correctly displaying the columns, you might want to use a spreadsheet or an addon for your
favourite text editor. When you use a spreadsheet such as Excel, it might annoy you by interpreting fractions as
dates. This can be circumvented by using `Data --> From Text/CSV` or the free alternative
[LibreOffice Calc](https://www.libreoffice.org/download/download/). Other than that, TSV data can be loaded with
every modern programming language.

### Loading TSV files in Python

Since the TSV files contain null values, lists, fractions, and numbers that are to be treated as strings, you may want
to use this code to load any TSV files related to this repository (provided you're doing it in Python). After a quick
`pip install -U ms3` (requires Python 3.10) you'll be able to load any TSV like this:

```python
import ms3

labels = ms3.load_tsv('harmonies/op01n01a.tsv')
notes = ms3.load_tsv('notes/op01n01a.tsv')
```

## Column names

You can look up meaning and data type of the columns of all TSV files including `metadata.tsv` in 
[ms3's documentation](https://johentsch.github.io/ms3/columns) (simply search through the page).


## Generating all TSV files from the scores

When you have made changes to the scores and want to update the TSV files accordingly, you can use the following
command (provided you have pip-installed [ms3](https://github.com/johentsch/ms3)):

```python
ms3 extract -M -N -X -D # for measures, notes, expanded annotations, and metadata
```

If, in addition, you want to generate the reviewed scores with out-of-label notes colored in red, you can do

```python
ms3 review -M -N -X -D # for extracting measures, notes, expanded annotations, and metadata
```

By adding the flag `-c` to the review command, it will additionally compare the (potentially modified) annotations in the score
with the ones currently present in the harmonies TSV files and reflect the comparison in the reviewed scores.

## Score origin

To create the dataset we downloaded the musicXML conversion available on Craig Sapp's
[KernScores](http://kern.humdrum.org/search?s=t&keyword=Corelli) (thanks to the engraver(s) who first encoded the 
scores in **kern format), converted them to MuseScore, and had them corrected and completed by the transcription service
[tunescribers.com](https://www.tunescribers.com/). This involved adding thorough bass figures throughout and 
engraving a few missing movements from scratch. The commission was performed based on the Pepusch prints available 
on the International Music Score Library Project (IMSLP) which are included in the folder ``pdf``:

| Opus | File                                        | IMSLP                                             |
|------|---------------------------------------------|---------------------------------------------------|
| 1    | Corelli op. 1 12 Triosonaten - Partitur.pdf | https://imslp.org/wiki/Special:ReverseLookup/1666 |
| 3    | Corelli op. 3 12 Triosonaten - Partitur.pdf | https://imslp.org/wiki/Special:ReverseLookup/1689 |
| 4    | Corelli op. 4 12 Triosonaten - Partitur.pdf | https://imslp.org/wiki/Special:ReverseLookup/1690 |

(The scan of op. 3 is missing page 46, corresponding to `op03n12a`)

Whenever pitches, bass figures or their placement were obviously wrong they have been corrected based on the 
Rome princeps editions.

## Caveats

### Wrong positions

Two files have different time signatures in the upper and lower staff pairs which leads to wrong positions:

* `op03n10d` has 12/8 vs. 2/2
* `op04n06g` has 12/8 vs. 4/4

Since the parser deals only with one time signature per measure, and since positions are computed additively,
the positions are currently incorrect for

* all events in these two pieces which
* occur in staff 3 or 4
* after beat 1.

As a remedy, staves 1 and 2 could be re-written in simple meters (2/2 or 4/4) sporting triplets. For now,
users could multiply `mc_onset` values for staves 3 and 4 by 1.5 as a remedy. The quarterbeats would then need to be
re-computed by adding the stretched onset values to the MC's quarterbeat.

### `.warnings` files

As long as such files exist in the `reviewed` folder, the `ms3 review` command has detected

* incongruent phrase beginnings `{` and endings `}`, and/or
* harmony labels where over 60 % of the note heads in the segment are out-of-label, and/or
* other warnings related to parsing the scores or annotations.

Pull requests addressing any of these warnings would be highly appreciated.

### Instruments

The information on the four parts in the MuseScore files has not been curated. That concerns the staff names,
brackets, behaviour of barlines, and instruments. If someone could send us a good configuration that looks and sounds
decent, we would be glad to automatically apply it to the entire dataset.

## Questions, Suggestions, Corrections, Bug Reports

Please [create an issue](https://github.com/DCMLab/corelli/issues) and/or feel free to fork and submit pull requests.

## License

Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License ([CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)).

![cc-by-nc-sa-image](https://licensebuttons.net/l/by-nc-sa/4.0/88x31.png)

## File naming convention

For example, all files starting with `op03n02` are movements of Sonata number 2 from opus 3. The sequence of movements
is indicated by appended letters `op03n02a`, `op03n02b`, etc.


## Overview
|file_name|measures|labels|standard|                            annotators                            |      reviewers       |
|---------|-------:|-----:|--------|------------------------------------------------------------------|----------------------|
|op01n01a |      14|    64|2.3.0   |Lars Opfermann, Ya-Chuan Wu (2.1.1), Hanné Becker (2.3.0)         |HB, JH                |
|op01n01b |      38|   141|2.3.0   |Lars Opfermann , Ya-Chuan Wu (2.1.1), Hanné Becker (2.3.0)        |HB, JH                |
|op01n01c |      37|    97|2.3.0   |Lars Opfermann and Ya-Chuan Wu (2.1.1), Hanné Becker (2.3.0)      |AN, HB                |
|op01n01d |      98|   147|2.3.0   |Lars Opfermann, Ya-Chuan Wu (2.1.1), Hanné Becker (2.3.0)         |HB, AN, JH            |
|op01n02a |      19|    76|2.3.0   |Lars Opfermann (2.1.1), Ya-Chuan Wu (2.1.1), Hanné Becker (2.3.0) |AN                    |
|op01n02b |      36|   121|2.3.0   |Lars Opfermann (2.1.1), Ya-Chuan Wu (2.1.1), Hanné Becker (2.3.0) |AN, HB                |
|op01n02c |      29|    67|2.3.0   |Lars Opfermann (2.1.1), Ya-Chuan Wu (2.1.1), Hanné Becker (2.3.0) |AN                    |
|op01n02d |     117|   174|2.3.0   |Lars Opfermann (2.1.1), Ya-Chuan Wu (2.1.1), Hanné Becker (2.3.0) |AN, HB                |
|op01n03a |      17|    66|2.3.0   |Lars Opfermann (2.1.1), Ya-Chuan Wu (2.1.1), Hanné Becker (2.3.0) |AN                    |
|op01n03b |      54|   223|2.3.0   |Lars Opfermann (2.1.1), Ya-Chuan Wu (2.1.1), Hanné Becker (2.3.0) |AN, HB                |
|op01n03c |      35|    71|2.3.0   |Lars Opfermann (2.1.1), Ya-Chuan Wu (2.1.1), Hanné Becker (2.3.0) |AN                    |
|op01n03d |      97|   177|2.3.0   |Lars Opfermann (2.1.1), Ya-Chuan Wu (2.1.1), Hanné Becker (2.3.0) |AN, HB                |
|op01n04a |      19|    52|2.3.0   |Lars Opfermann (2.1.1), Ya-Chuan Wu (2.1.1), Hanné Becker (2.3.0) |AN, HB                |
|op01n04b |      12|    26|2.3.0   |Lars Opfermann (2.1.1), Ya-Chuan Wu (2.1.1), Hanné Becker (2.3.0) |AN, HB                |
|op01n04c |      28|   117|2.3.0   |Lars Opfermann (2.1.1) , Ya-Chuan Wu(2.1.1), Hanné Becker (2.3.0) |AN, HB                |
|op01n04d |      39|   115|2.3.0   |Lars Opfermann (2.1.1), Ya-Chuan Wu (2.1.1), Hanné Becker (2.3.0) |AN                    |
|op01n05a |      42|    90|2.3.0   |Lars Opfermann (2.1.1), Ya-Chuan Wu (2.1.1), Hanné Becker (2.3.0) |AN                    |
|op01n05b |      39|   149|2.3.0   |Lars Opfermann (2.1.1), Ya-Chuan Wu (2.1.1), Hanné Becker (2.3.0) |AN                    |
|op01n05c |      30|    59|2.3.0   |Lars Opfermann (2.1.1), Ya-Chuan Wu (2.1.1), Hanné Becker (2.3.0) |AN                    |
|op01n05d |      59|   117|2.3.0   |Lars Opfermann (2.1.1), Ya-Chuan Wu (2.1.1), Hanné Becker (2.3.0) |AN                    |
|op01n06a |      11|    44|2.3.0   |Lars Opfermann, Ya-Chuan Wu (2.1.1), Victor Zheng (2.3.0)         |VZ, JH                |
|op01n06b |      40|   190|2.3.0   |Lars Opfermann, Ya-Chuan Wu, Victor Zheng (2.3.0)                 |VZ, JH                |
|op01n06c |      38|    89|2.3.0   |Lars Opfermann, Ya-Chuan Wu, Victor Zheng (2.3.0)                 |VZ, JH                |
|op01n06d |      69|   131|2.3.0   |Lars Opfermann, Ya-Chuan Wu, Victor Zheng (2.3.0)                 |VZ, JH                |
|op01n07a |      40|   161|2.3.0   |Kristine Kier Jørgensen (2.1.1), Victor Zheng (2.3.0)             |VZ, JH                |
|op01n07b |      14|    57|2.3.0   |Cristiana Palandri (2.1.1), Victor Zheng (2.3.0)                  |VZ, AN, JH            |
|op01n07c |      51|   174|2.3.0   |Cristiana Palandri (2.1.1), Victor Zheng (2.3.0)                  |VZ, AB                |
|op01n08a |      16|    56|2.3.0   |Lars Opfermann, Ya-Chuan Wu (2.1.1), Ehsan Mohagheghi Fard (2.3.0)|EMF, DK               |
|op01n08b |      20|    76|2.3.0   |Lars Opfermann, Ya-Chuan Wu (2.1.1), Ehsan Mohagheghi Fard (2.3.0)|EMF, DK               |
|op01n08c |      27|   119|2.3.0   |Lars Opfermann, Ya-Chuan Wu (2.1.1), Ehsan Mohagheghi Fard (2.3.0)|EMF, DK               |
|op01n08d |      39|    71|2.3.0   |Lars Opfermann, Ya-Chuan Wu (2.1.1), Ehsan Mohagheghi Fard (2.3.0)|EMF, DK               |
|op01n09a |      39|    45|2.3.0   |Lars Opfermann, Ya-Chuan Wu (2.1.1), Ehsan Mohagheghi Fard (2.3.0)|EMF, DK               |
|op01n09b |      37|   153|2.3.0   |Lars Opfermann, Ya-Chuan Wu (2.1.1), Ehsan Mohagheghi Fard (2.3.0)|EMF, VZ               |
|op01n09c |      36|    74|2.3.0   |Lars Opfermann, Ya-Chuan Wu (2.1.1), Ehsan Mohagheghi Fard (2.3.0)|EMF, DK               |
|op01n09d |      54|    56|2.3.0   |Lars Opfermann, Ya-Chuan Wu (2.1.1), Ehsan Mohagheghi Fard (2.3.0)|EMF, DK               |
|op01n10a |      12|    45|2.3.0   |Lars Opfermann, Ya-Chuan Wu (2.1.1), Amelia Brey (2.3.0)          |AB, AN                |
|op01n10b |      17|    70|2.3.0   |Lars Opfermann, Ya-Chuan Wu (2.1.1), Amelia Brey (2.3.0)          |AB, AN                |
|op01n10c |      29|   107|2.3.0   |Lars Opfermann, Ya-Chuan Wu (2.1.1), Amelia Brey (2.3.0)          |AB, AN                |
|op01n10d |      24|    56|2.3.0   |Lars Opfermann, Ya-Chuan Wu (2.1.1), Amelia Brey (2.3.0)          |AB, AN                |
|op01n10e |      84|   139|2.3.0   |Lars Opfermann, Ya-Chuan Wu (2.1.1), Amelia Brey (2.3.0)          |AB, AN                |
|op01n11a |      18|    60|2.3.0   |Hanné Becker                                                      |JH                    |
|op01n11b |      34|   123|2.3.0   |Hanné Becker                                                      |JH                    |
|op01n11c |      33|    79|2.3.0   |Hanné Becker                                                      |AN                    |
|op01n11d |      18|    81|2.3.0   |Hanné Becker                                                      |AN                    |
|op01n12a |      19|    64|2.3.0   |Ehsan mohagheghi Fard                                             |AN                    |
|op01n12b |      36|   101|2.3.0   |Ehsan Mohagheghi Fard                                             |AN                    |
|op01n12c |      13|    42|2.3.0   |Tomoko Ono                                                        |JH                    |
|op01n12d |      62|   225|2.3.0   |Ehsan Mohagheghi Fard                                             |AN                    |
|op03n01a |      19|    78|2.3.0   |Gabriela Ortiz Würth (2.1.1), Adrian Nagel (2.3.0)                |AN (2.1.0), AW (2.3.0)|
|op03n01b |      37|   149|2.3.0   |Gabriela Ortiz Würth (2.1.1), Adrian Nagel (2.3.0)                |AN, AW                |
|op03n01c |      61|   117|2.3.0   |Gabriela Ortiz Würth (2.1.1), Adrian Nagel (2.3.0)                |AN (2.1.1), AW (2.3.0)|
|op03n01d |      40|   158|2.3.0   |Gabriela Ortiz Würth (2.1.1), Adrian Nagel (2.3.0)                |AN (2.1.1), AW (2.3.0)|
|op03n02a |      19|    68|2.3.0   |Moritz Heffter (2.1.1), Amelia Brey (2.3.0)                       |AB, ST                |
|op03n02b |      31|   118|2.3.0   |Moritz Heffter (2.1.1), Amelia Brey (2.3.0)                       |AB, ST                |
|op03n02c |      40|    91|2.3.0   |Moritz Heffter (2.1.1), Amelia Brey (2.3.0)                       |AB, ST                |
|op03n02d |      43|   121|2.3.0   |Moritz Heffter (2.1.1), Amelia Brey (2.3.0)                       |AB, ST                |
|op03n03a |      16|    59|2.3.0   |Moritz Heffter (2.1.1), Amelia Brey (2.3.0)                       |AB                    |
|op03n03b |      32|    62|2.3.0   |Moritz Heffter (2.1.1), Amelia Brey (2.3.0)                       |AB, ST                |
|op03n03c |      23|   137|2.3.0   |Moritz Heffter (2.1.1), Amelia Brey (2.3.0)                       |AB, ST                |
|op03n03d |      60|   240|2.3.0   |Moritz Heffter (2.1.1), Amelia Brey (2.3.0)                       |AB, ST                |
|op03n04a |      23|   137|2.3.0   |Kelsey Lussier                                                    |JH                    |
|op03n04b |      39|   138|2.3.0   |Ehsan Mohagheghi Fard                                             |DK                    |
|op03n04c |      55|   114|2.3.0   |Yannis Rammos                                                     |JH                    |
|op03n04d |      50|   171|2.3.0   |Ehsan Mohagheghi Fard                                             |AB                    |
|op03n05a |      21|    89|2.3.0   |Mastaneh Nazarian (2.1.1), John Heilig (2.3.0)                    |JH                    |
|op03n05b |      49|   175|2.3.0   |John Heilig                                                       |AN                    |
|op03n05c |      36|    80|2.3.0   |John Heilig                                                       |AN                    |
|op03n05d |      33|   125|2.3.0   |John Heilig                                                       |AN                    |
|op03n06a |      33|   119|2.3.0   |Kyle Quarles                                                      |AN                    |
|op03n06b |      14|    54|2.3.0   |Kyle Quarles                                                      |JH                    |
|op03n06c |      38|   150|2.3.0   |Kyle Quarles                                                      |AN                    |
|op03n06d |      41|   179|2.3.0   |Ehsan Mohagheghi Fard                                             |DK                    |
|op03n07a |      20|    76|2.3.0   |Hanné Becker                                                      |JH                    |
|op03n07b |      35|   131|2.3.0   |Hanné Becker                                                      |AN                    |
|op03n07c |      38|    92|2.3.0   |Hanné Becker                                                      |AN                    |
|op03n07d |      28|    84|2.3.0   |Hanné Becker                                                      |AN                    |
|op03n08a |      20|    90|2.3.0   |Matthew Chiu                                                      |JH                    |
|op03n08b |      39|   153|2.3.0   |Ehsan Mohagheghi Fard                                             |DK                    |
|op03n08c |      31|    74|2.3.0   |Matthew Chiu                                                      |AN                    |
|op03n08d |      40|   135|2.3.0   |Matthew Chiu                                                      |AN                    |
|op03n09a |      30|    69|2.3.0   |John Heilig                                                       |JH                    |
|op03n09b |      28|   103|2.3.0   |John Heilig                                                       |AN                    |
|op03n09c |      38|    85|2.3.0   |John Heilig                                                       |AN                    |
|op03n09d |      28|   121|2.3.0   |John Heilig                                                       |AN                    |
|op03n10a |      14|    49|2.3.0   |Tal Soker                                                         |JH                    |
|op03n10b |      38|   154|2.3.0   |Tal Soker                                                         |JH                    |
|op03n10c |      11|    25|2.3.0   |Tal Soker                                                         |JH                    |
|op03n10d |      31|   114|2.3.0   |Tal Soker                                                         |JH                    |
|op03n11a |      17|    68|2.3.0   |John Heilig                                                       |JH                    |
|op03n11b |      39|   164|2.3.0   |John Heilig                                                       |AN                    |
|op03n11c |      29|    63|2.3.0   |John Heilig                                                       |AN                    |
|op03n11d |      45|    97|2.3.0   |John Heilig                                                       |JH                    |
|op03n12a |      19|    17|2.3.0   |John Heilig                                                       |JH                    |
|op03n12b |      41|     8|2.3.0   |John Heilig                                                       |AN                    |
|op03n12c |       9|    39|2.3.0   |John Heilig                                                       |AN                    |
|op03n12d |      29|    97|2.3.0   |John Heilig                                                       |AN                    |
|op03n12e |      25|    76|2.3.0   |John Heilig                                                       |AN                    |
|op03n12f |      37|   192|2.3.0   |John Heilig                                                       |AN                    |
|op03n12g |      46|   205|2.3.0   |John Heilig                                                       |AN                    |
|op04n01a |      17|    74|2.3.0   |Lydia Carlisi (2.1.1), Adrian Nagel (2.3.0)                       |AN, JH                |
|op04n01b |      47|    75|2.3.0   |Lydia Carlisi (2.1.1), Adrian Nagel (2.3.0)                       |AN, JH                |
|op04n01c |      13|    47|2.3.0   |Lydia Carlisi (2.1.1), Adrian Nagel (2.3.0)                       |AN, JH                |
|op04n01d |      34|   136|2.3.0   |Lydia Carlisi (2.1.1), Adrian Nagel (2.3.0)                       |AN, JH                |
|op04n02a |      20|    87|2.3.0   |Lydia Carlisi (2.1.1), Adrian Nagel (2.3.0)                       |AN, JH                |
|op04n02b |      22|   111|2.3.0   |Lydia Carlisi (2.1.1), Adrian Nagel (2.3.0)                       |AN, JH                |
|op04n02c |       5|     9|2.3.0   |Lydia Carlisi (2.1.1), Adrian Nagel (2.3.0)                       |AN, JH                |
|op04n02d |      57|   125|2.3.0   |Lydia Carlisi (2.1.1), Adrian Nagel (2.3.0)                       |AN, JH                |
|op04n03a |      18|    90|2.3.0   |Lydia Carlisi (2.1.1), Adrian Nagel (2.3.0)                       |AN, DK                |
|op04n03b |      48|    87|2.3.0   |Lydia Carlisi (2.1.1), Adrian Nagel (2.3.0)                       |AN, DK                |
|op04n03c |      16|    37|2.3.0   |Lydia Carlisi (2.1.1), Adrian Nagel (2.3.0)                       |AN, DK                |
|op04n03d |      41|   136|2.3.0   |Lydia Carlisi (2.1.1), Adrian Nagel (2.3.0)                       |AN, DK                |
|op04n04a |      20|    68|2.3.0   |Lydia Carlisi (2.1.1), Adrian Nagel (2.3.0)                       |AN, DK                |
|op04n04b |      41|    80|2.3.0   |Lydia Carlisi (2.1.1), Adrian Nagel (2.3.0)                       |AN, DK                |
|op04n04c |      17|   102|2.3.0   |Lydia Carlisi (2.1.1), Adrian Nagel (2.3.0)                       |AN, DK                |
|op04n04d |      31|   107|2.3.0   |Lydia Carlisi (2.1.1), Adrian Nagel (2.3.0)                       |AN, AB                |
|op04n05a |      20|   101|2.3.0   |Andrew Wilson                                                     |JH                    |
|op04n05b |      28|   105|2.3.0   |Andrew Wilson                                                     |JH                    |
|op04n05c |      39|    78|2.3.0   |Andrew Wilson                                                     |JH                    |
|op04n05d |      12|    38|2.3.0   |Andrew Wilson                                                     |AN                    |
|op04n06a |       6|    22|2.3.0   |Adrian Nagel                                                      |Victor Zheng          |
|op04n06b |      23|    41|2.3.0   |Adrian Nagel                                                      |Victor Zheng          |
|op04n06c |       7|    19|2.3.0   |Adrian Nagel                                                      |Victor Zheng          |
|op04n06d |      28|    57|2.3.0   |Adrian Nagel                                                      |Victor Zheng          |
|op04n06e |      10|    22|2.3.0   |Adrian Nagel                                                      |Victor Zheng          |
|op04n06f |      25|    76|2.3.0   |Tim Girard                                                        |AN                    |
|op04n06g |      30|    94|2.3.0   |Justin Franklin                                                   |JH                    |
|op04n07a |      15|    86|2.3.0   |Justin Franklin                                                   |JH                    |
|op04n07b |      46|   104|2.3.0   |Justin Franklin                                                   |AN                    |
|op04n07c |       6|    12|2.3.0   |Justin Franklin                                                   |JH                    |
|op04n07d |      32|    47|2.3.0   |Justin Franklin                                                   |JH                    |
|op04n07e |      27|    92|2.3.0   |Justin Franklin                                                   |AN                    |
|op04n08a |      38|    94|2.3.0   |Kevin Davis                                                       |AN, JH                |
|op04n08b |      23|    88|2.3.0   |Kevin Davis                                                       |JH                    |
|op04n08c |      16|    41|2.3.0   |Kevin Davis                                                       |AN, JH                |
|op04n09a |      13|    77|2.3.0   |Victor Zheng                                                      |AN                    |
|op04n09b |      44|   103|2.3.0   |Victor Zheng                                                      |AN                    |
|op04n09c |      12|    49|2.3.0   |Victor Zheng                                                      |JH                    |
|op04n09d |      56|   167|2.3.0   |Victor Zheng                                                      |JH                    |
|op04n10a |       2|     4|2.3.0   |Kevin Davis                                                       |JH                    |
|op04n10b |      33|   104|2.3.0   |Kevin Davis                                                       |JH                    |
|op04n10c |       4|     8|2.3.0   |Kevin Davis                                                       |AN, JH                |
|op04n10d |      14|    51|2.3.0   |Kevin Davis                                                       |AN, JH                |
|op04n10e |      48|   152|2.3.0   |Kevin Davis                                                       |AN JH                 |
|op04n11a |      24|   156|2.3.0   |Amelia Brey                                                       |AN                    |
|op04n11b |      73|   116|2.3.0   |Amelia Brey                                                       |AN                    |
|op04n11c |      36|   142|2.3.0   |Amelia Brey                                                       |AN                    |
|op04n12a |      35|    77|2.3.0   |Andrew Wilson                                                     |AN                    |
|op04n12b |      39|   111|2.3.0   |Andrew Wilson                                                     |AN                    |
|op04n12c |      19|    62|2.3.0   |Andrew Wilson                                                     |AN                    |


*Overview table automatically updated using [ms3](https://ms3.readthedocs.io/).*
