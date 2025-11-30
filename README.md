# OpenKnot AI Design Data
**Chemical mapping data for AI and Eterna designs from OpenKnot challenge (2024-2025)**

OpenKnotBench data release,
previously called Eterna Pseudoknot 17 (EPK-17) 


## Data description

The data are in CSV format similar to the Kaggle Ribonanza competition, with the following fields:

- `id` - (string) An arbitrary identifier, typically including tags to define which of 17 secondary structure puzzles (e.g., `W01` for week 1), internal ID from the designer, and some information on how the sequence was padded on 5’ and 3’ ends.
- `sequence` - (string) Describes the RNA sequence, a string of `A`, `C`, `G`, and `U`. Includes 5’ and 3’ pad sequences used to aid experimental characterization.
- `experiment_type` - (string) Description of the type of chemical mapping experiment that was used to generate each profile (currently only `2A3_MaP`, a SHAPE experimental method).
- `dataset_name` - (string) arbitrary name of high throughput sequencing dataset from which the reactivity profile was extracted.
- `reads` - (integer) Number of reads in the high throughput sequencing experiment that were assigned to the RNA sequence, and whose mutations were tabulated to compile the reactivity profile. 
- `signal_to_noise` - (float) Signal/noise value for the profile, defined as mean(measurement value over probed positions)/mean( statistical error in measurement value over probed positions). Probed positions are those positions that are not `null` in `reactivity` or `reactivity_error`, that have non-zero `reactivity_error`; the very first and last such position in any given profile are also dropped for calculating signal_to_noise. (These values do not need to be predicted in this competition.)
- `SN_filter` - (Boolean) 0 or 1 depending on whether the profile has `signal_to_noise`> 1.00 and `reads`> 100. A filter used in Ribonanza to define a ‘reasonable’ level of signal/noise.
- `round` - (integer) which round of the Eterna Pseudoknot 17 benchmark study (1 or 2). Field added in v2.0.0.
- `puzzle` - (string) which of the 17 puzzles the design was submitted for (e.g., `W01`). Field added in v1.1.0.
- `method` - (string) which design method was used for the sequence (e.g., `Eterna`). Field added in v1.1.0.
- `target_openknot_score` - (float) number between 0 and 100 that scores percentage agreement between design’s SHAPE reactivity profile and the target structure. Target unpaired residues are counted as correct if their reactivity is greater than 0.125; target paired residues should have reactivity less than 0.25. A second subscore is computed over just residues in targeted ‘crossed’ pairs [those pairs (`i,j`) where there is at least one other pair (`m,n`) with `m<i<n<j` or `i<m<j<n`]; the two scores are then averaged to give the final score. For both subscores, only computed over residues corresponding to the submitted design, not flanking padded regions added to aid experiments. Field added in v1.1.0.
- `sub_start` - (integer) position in full sequence at which the submitted design starts. Field added in v1.1.0.
- `sub_end` - (integer) position in full sequence at which the submitted design ends. Field added in v1.1.0.
ref_structure - (string) target secondary structure in dot-bracket notation, same length as full sequence (includes pads). Field added in v1.1.0.
- `design_length` - (integer) length of just the designed sequence, without padding or barcodes added to enable experimental characterization. Field added in v4.2.0.
- `design_sequence` -  just the designed sequence, without padding or barcodes added to enable experimental characterization. Field added in v4.2.0.
- `target_structure` - target secondary structure in dot-bracket notation which the design should match
- `RNet_structure` - predicted secondary structure from RibonanzaNet (RNet). Field added in v4.2.0.
- `RNet_F1` - harmonic mean of precision and recall of base pairs for `RNet_structure` compared to `target_structure`. Field added in v4.2.0.
- `RNet-F1_crossed_pair` - harmonic mean of precision and recall of just crossed base pairs for `RNet_structure` compared to `target_structure`. Crossed pairs are those pairs (` i,j `) where there is at least one other pair (` m,n `) with `m<i<n<j` or `i<m<j<n`. Field added in v4.2.0.
- `reactivity_0001`, `reactivity_0002`,… - (float) An array of floating point numbers, should have the same length as the RNA sequence, which defines the reactivity profile for the RNA. Several positions near the beginning and end of the sequence cannot be probed due to technical reasons, and their reactivity values are `null`. The values should be greater than or equal to zero, but due to experimental errors can become negative. The values are normalized so that the 90th percentile value within the larger dataset is 1.0.
- `reactivity_error_0001`, `reactivity_error_0002`,… - (float) An array of floating point numbers, should have the same length as the corresponding `reactivity_*` columns, calculated errors in experimental values obtained in reactivity derived from counting statistics in the high-throughput sequencing experiment. 


## Release notes


### [v4.2.0](v4.2.0) (30 November, 2025) 
- Data on ‘round 4’ designs on 20 240-mer puzzles (collected in Eterna’ [OpenKnot Round 7b lab puzzles](https://eternagame.org/labs/13612324)), including puzzles like `Q07` (`‘Broad_bean_mottle_virus’`) have been rescued and added.
- Data on `struct2seq` designs and `codesign-RFdiff` for 'round 4' have also been added.
- New columns `target_structure`, `RNet_structure`, `RNet_F1`, `RNet_F1_crossed_pair` have been added to allow filtering of designs whose SHAPE profiles match the target profiles but with the wrong secondary structures, as appears to have occurred in some 'round 3' designs, as evaluated by mutate-map-rescue experiments (to be released shortly). 

### [v3.1.0](v3.1.0) (23 March, 2025) 
- Experiments for ‘round 4’ designs on 20 240-mer puzzles (collected in Eterna’ [OpenKnot Round 7b lab puzzles](https://eternagame.org/labs/13612324)) are appended to the file. Tab of the [OpenKnot 7 sheet](https://docs.google.com/spreadsheets/d/14su6H6qmXT0xu6WZ4XtBnQMea5_8t2iG0gyf3wd2eWg/edit?gid=2041530679#gid=2041530679) compiles structures and starter sequences. The 20 new puzzles are labeled `Q01`, … `Q20`.
- For a few of the puzzles, (notable `Q07`, `‘Broad_bean_mottle_virus’`), there were few data with good reads due to an oversight in padding the constructs to fixed length. When comparing design methods, it is best to continue to ignore any designs with experimental `SN_filter`=0.
- Note that the data set is ‘wider’ (more `reactivity_0001`, `reactivity_0002`,… and `reactivity_error_0001`, `reactivity_error_0002`,… columns) to accommodate the longer RNA’s that host the 240-mers. For the prior shorter puzzles, these extra nucleotides are assigned reactivities of `null`.

### [v3.0.0](v3.0.0) (29 Jan, 2025)
- New experiments for ‘round 2’ designs on EPK-17 puzzles (collected in [Eterna’s OpenKnot Round 6 lab puzzle](https://eternagame.org/labs/13389097)), resolving cDNA bottleneck that increased noise for round 2 in v2.0.x releases.
- New experiments for ‘round 3’ designs on 20 100-mer puzzles (collected in [Eterna’s OpenKnot Round 7a lab puzzle](https://eternagame.org/labs/13612323)). See [this sheet](https://docs.google.com/spreadsheets/d/14su6H6qmXT0xu6WZ4XtBnQMea5_8t2iG0gyf3wd2eWg/edit?gid=1554464096#gid=1554464096) for compilation of structures and starter sequences, and [this folder](https://drive.google.com/drive/folders/1dnWqZ5lfubics1oAmmSkw_y8Q_Qg4tD5) for provided starter PDB files.
- The 20 puzzles of round 3 are labeled `P01`, ... `P20`; and the 17 puzzles in rounds 1 and 2 are labeled `W01`,... `W17`.


### [v2.0.2](v2.0.2) (27 Oct, 2024)
- Bugfix in `ref_structure` for ~10 designs.
- Bugfix in `reads` for all round 2 designs. `SN_filter` does not change.

### [v2.0.0](v2.0.0) (27 Oct, 2024)
- Collection of data on 8711 additional submissions for the EPK-17 puzzles – a ‘round 2’ of designs. 
- Sequences were collected on [Eterna’s OpenKnot Round 6 lab puzzle](https://eternagame.org/labs/13389097).
- The data are appended to the prior v1.2.0 file, which are ‘round 1’. A new ‘round’ field has been added to the .csv data file.
- Caveat: Repeats of wild type sequences for some puzzles are noisier in round 2 experiments than round 1, leading to drop in WT scores most notably for `W10` and `W15`. Studies are continuing to track down the origin of this increased noise.

### [v1.2.0](v1.2.0) (15 Aug, 2024)
- Integration of a second large Illumina data set where sequences were rebalanced so that sequences that previously dropped out receive more reads. Now >99% of the sequences have 2A3 profiles with `signal_to_noise` > 1.0.

### [v1.1.0](v1.1.0) (12 August, 2024) 
- Reassignment and disambiguation of some MPNN sequences whose lengths did not match the target secondary structures, based on additional information from submitters. This step leads to inclusion of 35 more sequences (total of 8595) that were previously filtered out.
- Added fields for puzzle, design method, Eterna-based secondary structure score (target OpenKnot Score), reference structures, and residue numbers of submission sequence within the full, padded sequence.

### [v1.0.0](v1.0.0) (8 August, 2024) 
- Initial SHAPE data release for 8,560 sequences, based on a NovaSeq run with experimental protocol from Ribonanza ([link](https://www.biorxiv.org/content/10.1101/2024.02.24.581671v2)). 
- RNA’s were probed with the SHAPE reagent 2A3 which typically acylates nucleotides that are not forming pairs.
- Designs come from Eterna OpenKnot 4 challenges, and expert laboratories applying automated computational design methods, to [17 pseudoknotted secondary structures](https://docs.google.com/spreadsheets/d/1Ps4rRnhCpUszLSD9ydomN2JjJ3QuFVC4YWVszl3iQ3Y/edit?gid=0#gid=0).
- Data acquired by W. Kladwang with analysis from R. Das and sequence preparation by H.M. Blair and Eterna team. 
- These data are part of a larger set of 15,000 sequences that was nicknamed OK45 or DO (‘dial-out’) internally in the Das lab at Stanford.
- More data, focused on sequences that largely drop out of conventional protocol through a dial-out PCR strategy, are coming in, expected later in August.


