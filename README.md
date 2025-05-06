# 🧬 CrossFoldDB
**CrossFoldDB** is a framework for performing and storing precomputed **FoldSeek protein structure alignments** across related species and reference outgroups. 

---

## 🔍 Key Features

- Precomputed all-vs-all structure-based protein alignments using FoldSeek
- Forward and reverse queries to ensure completeness
- Custom annotation integration from UniProt and genome metadata
- Supports large-scale parallel searches on HPC
- JSON conversion pipeline for downstream visualization and filtering

---

## 🧪 Use Case Examples

### 🦠 *Aspergillus flavus*

Structure-based homology to *Aspergillus parasiticus* and *Aspergillus hiratsukae**Arabidopsis* Outgroup filtering helps isolate Aspergillus-specific structures.  Outgroup species in this example are  *Fusarium graminearum* and *Saccharomyces cerevisiae*.


---

## 📁 Directory Structure


```text
<pre><code>
CrossFoldDB/
├── structures/ # Compressed AlphaFold/CIF inputs per species
│ └── <species>/
|
├── DB/ # FoldSeek formatted searchable databases
│ └── <species>DB/
|
├── html/ # Output HTML with alignment summaries
|
├── JSON/ # Parsed JSON results (post-filtered)
|
├── scripts/ # bash scripts
|   ├── species_list.txt # List of species to include in pipeline
|   ├── S1_buildFoldSeekDB.sh # Build script for FoldSeek databases
|   ├── S2_searchFoldSeek_parallel.sh # Parallel search script (forward/reverse)
|   ├── S3_extractJSON_parallel.sh # Convert FoldSeek results to legacy format
|   ├── S4_merge_JSON.sh # Combine and filter results
|   ├── S5_make_annotations.sh # Add annotations for reference proteins
|   ├── untar_directory.sh # Utility to unzip input CIFs in parallel
|
├── python/ # python scripts
|   ├──
|
└── README.md
```


---

## 🧰 Installation

Requirements:

- [FoldSeek](https://github.com/steineggerlab/foldseek)
- Python 3.8+
- SLURM-based HPC environment with `sbatch`

---

## 🚀 Quickstart

### 1. Unpack structure files

```bash
sbatch untar_directory.sh ./structures/arabidopsis
sbatch untar_directory.sh ./structures/graminearum
sbatch untar_directory.sh ./structures/cerevisiae
sbatch untar_directory.sh ./structures/sorghi
sbatch untar_directory.sh ./structures/bombycis
sbatch untar_directory.sh ./structures/candidus
sbatch untar_directory.sh ./structures/flavus
sbatch untar_directory.sh ./structures/hiratsukae
sbatch untar_directory.sh ./structures/parasiticus
sbatch untar_directory.sh ./structures/ruber
```

### 2. Create a species list
```bash
echo -e "arabidopsis\ngraminearum\ncerevisiae\nsorghi\nbombycis\ncandidus\nflavus\nhiratsukae\nparasiticus\nruber" > species_list.txt
```


### 3. Build FoldSeek databases
```bash
sbatch S1_buildFoldSeekDB.sh ./structures/arabidopsis/arabidopsis ./DB/arabidopsisDB
sbatch S1_buildFoldSeekDB.sh ./structures/graminearum/graminearum ./DB/graminearumDB
sbatch S1_buildFoldSeekDB.sh ./structures/cerevisiae/cerevisiae ./DB/cerevisiaeDB
sbatch S1_buildFoldSeekDB.sh ./structures/sorghi/sorghi ./DB/sorghiDB
sbatch S1_buildFoldSeekDB.sh ./structures/bombycis/bombycis ./DB/bombycisDB
sbatch S1_buildFoldSeekDB.sh ./structures/candidus/candidus ./DB/candidusDB
sbatch S1_buildFoldSeekDB.sh ./structures/flavus/flavus ./DB/flavusDB
sbatch S1_buildFoldSeekDB.sh ./structures/hiratsukae/hiratsukae ./DB/hiratsukaeDB
sbatch S1_buildFoldSeekDB.sh ./structures/parasiticus/parasiticus ./DB/parasiticusDB
sbatch S1_buildFoldSeekDB.sh ./structures/ruber/ruber ./DB/ruberDB
```

### 4. Run FoldSeek searches (forward and reverse)
Repeat forward and reverse searches for each species. Continue until the number of output JSON files stabilizes (≈ number of reference structures).
```bash
sbatch S2_searchFoldSeek_parallel.sh arabidopsis forward
sbatch S2_searchFoldSeek_parallel.sh graminearum forward
sbatch S2_searchFoldSeek_parallel.sh cerevisiae forward
sbatch S2_searchFoldSeek_parallel.sh sorghi forward
sbatch S2_searchFoldSeek_parallel.sh bombycis forward
sbatch S2_searchFoldSeek_parallel.sh candidus forward
sbatch S2_searchFoldSeek_parallel.sh flavus forward
sbatch S2_searchFoldSeek_parallel.sh hiratsukae forward
sbatch S2_searchFoldSeek_parallel.sh parasiticus forward
sbatch S2_searchFoldSeek_parallel.sh ruber forward

sbatch S2_searchFoldSeek_parallel.sh arabidopsis reverse
sbatch S2_searchFoldSeek_parallel.sh graminearum reverse
sbatch S2_searchFoldSeek_parallel.sh cerevisiae reverse
sbatch S2_searchFoldSeek_parallel.sh sorghi reverse
sbatch S2_searchFoldSeek_parallel.sh bombycis reverse
sbatch S2_searchFoldSeek_parallel.sh candidus reverse
sbatch S2_searchFoldSeek_parallel.sh flavus reverse
sbatch S2_searchFoldSeek_parallel.sh hiratsukae reverse
sbatch S2_searchFoldSeek_parallel.sh parasiticus reverse
sbatch S2_searchFoldSeek_parallel.sh ruber reverse

```

### 5. Convert JSON results to legacy format
```bash
sbatch S3_extractJSON_parallel.sh
```

### 6. Merge and filter JSON outputs
```bash
sbatch S4_merge_JSON.sh
```

### 7. Generate reference annotations (optional)
```bash
sbatch S5_make_annotations.sh  # (for flavus and parasiticus)
```
## 🌍 Species Included


### Reference 
- *Aspergillus flavus*

### Outgroups
- *Fusarium graminearum*
- *Saccharomyces cerevisiae*

### Focal *Aspergillus* Species
- *Aspergillus parasiticus*
- *Aspergillus hiratsukae*

## 📫 Citation & Contact
If you use CrossFoldDB in your research, please cite this repository.

For questions, issues, or collaborations, contact the MaizeGDB team or submit a GitHub issue.


