# Rubella Virus (RuV) Washington-Focused Builds (E1 and full genome)


## Build Overview
- **Build Name**: Rubella – Washington-focused E1 and full-genome builds 
- **Pathogen/Strain**: Rubella virus (RuV), genus *Rubivirus*, family *Matonaviridae* 
- **Scope**:  
These builds generate Washington-focused phylogenetic analyses for both the E1 genotyping window and the full viral genome, integrating U.S., North American, and minimal global genotype context to support case investigations, outbreak analysis, and CRS risk assessment. 

- **Purpose**:   This repository contains the Washington-focused Nextstrain builds for rubella virus, adapted from the core Nextstrain rubella workflow. These builds prioritize inclusion of all available Washington sequences (subject to quality and length filters) while retaining U.S., North American, and global genotype reference sequences to support outbreak investigation, genotype tracking, and congenital rubella syndrome (CRS) risk assessment.  
  Unlike the global Nextstrain rubella build, which uses a broadly representative subsampling scheme, this build is tuned to:
  - always retain Washington state sequences, and  
  - enrich for North American context and WHO reference genotypes to support jurisdictional public-health questions.

*Note: At present, only a small number of Washington rubella sequences exist in the upstream dataset. This build is designed to automatically include additional WA sequences if they are added in future releases.*
- **Nextstrain Build/s Location/s**: 
  - **Whole genome**:  
    `https://nextstrain.org/groups/wadoh/rubella/genome`
  - **E1 gene**:  
    `https://nextstrain.org/groups/wadoh/rubella/E1`

## Table of Contents
- [Pathogen Epidemiology](#pathogen-epidemiology)
- [Scientific Decisions](#scientific-decisions)
- [Getting Started](#getting-started)
  - [Data Sources & Inputs](#data-sources--inputs)
  - [Setup & Dependencies](#setup--dependencies)
    - [Installation](#installation)
    - [Clone the repository](#clone-the-repository)
- [Run the Build](#run-the-build-with-test-data)
  - [Expected Outputs](#expected-outputs)
  - [Visualizing Results](#visualize-results)
- [Customization for Local Adaptation](#customization-for-local-adaptation)
- [Contributing](#contributing)
- [License](#license)
- [Acknowledgements](#acknowledgements)


## Pathogen Epidemiology

### Overview
- **Pathogen type & family***: Rubella virus (RuV) is an enveloped, positive-sense, single-stranded RNA virus in the family *Matonaviridae*.
- **Key genotypes / lineages**: Rubella is classified into multiple genotypes (e.g., 1E, 1G, 2B, etc.). The build focuses on genotypes defined by WHO using the E1 gene genotyping window.
- **Transmission**: Primarily respiratory droplet transmission. Vertical transmission during pregnancy (maternal infection) can result in **congenital rubella infection** and **congenital rubella syndrome (CRS)**.

### Nomenclature
Globally, rubella surveillance primarily relies on sequencing a 739-nt region of the E1 gene because whole-genome sequencing is uncommon. This build supports both whole-genome and E1 analyses for backward compatibility with WHO genotyping workflows.

### Geographic Distribution and Seasonality
- Rubella is globally distributed but substantially reduced in regions with robust vaccination programs.  
- In the U.S., endemic rubella transmission has been eliminated, with most cases associated with:
  - importations,  
  - limited local transmission chains, and  
  - sporadic CRS cases.  
- Seasonal patterns are less pronounced in settings with high vaccination coverage but may still appear in under-immunized communities.

### Public Health Importance
- Rubella infection in early pregnancy can cause **severe fetal outcomes**, including CRS (hearing loss, cardiac defects, cataracts, neurodevelopmental issues).  
- Surveillance supports:
  - verification of continued elimination,  
  - rapid investigation of importations,  
  - detection of transmission clusters, and  
  - characterization of genotypes associated with CRS or outbreaks.

### Genomic Relevance
Genomic data for rubella are particularly useful for:
- **Genotype assignment** using the E1 gene window.  
- **Outbreak investigations**, including:
  - resolving whether cases are linked,  
  - distinguishing multiple introductions vs. sustained transmission.  
- **Contextualizing CRS cases** by linking infant and maternal sequences and placing them within global genotype diversity.  
- **Verifying elimination** by showing that detected sequences remain consistent with sporadic importations rather than persistent endemic lineages.

### Additional Resources
- WHO rubella and CRS resources (case definitions, genotyping guidelines). `https://www.who.int/publications/i/item/9789290619734`
- CDC rubella and CRS technical pages.  `https://www.cdc.gov/surv-manual/php/table-of-contents/chapter-15-congenital-rubella-syndrome.html`


## Scientific Decisions
The Rubella Washington-focused Nextstrain build was designed to balance local public-health relevance, North American epidemiologic context, and global genotype interpretability, while avoiding excessive over-sampling from regions with dense data (e.g., Japan). Below are the key scientific choices made during development of this build and the rationale behind them.

Nextstrain builds are created with specific public-health goals in mind. For rubella, the key priorities were:
(1) retain all Washington sequences,
(2) ensure appropriate U.S. and North America context,
(3) preserve essential global genotype references, and
(4) support E1-based WHO genotyping as well as full-genome phylogenetics used for outbreak investigations and CRS assessments.

The following are critical decisions that were made during the development of this build that should be kept in mind when analyzing the data and using this build. *Subsampling, root selection, and reference selection must be included at minimum.*

### Nomenclature
- **Genotype / clade labels**:
  - WHO rubella genotypes (e.g., 1E, 2B) are used where possible.
  - If Nextclade support is added upstream in the future, additional clade labels may be exposed.
- **Metadata fields**:
  - `clade` is used for genotype/clade coloring in Auspice.
  - `genbank_genotype` (when available) is retained for comparison with other annotations.

### Subsampling 
Rubella sequence availability is extremely uneven globally, and Washington currently has very few sequences represented in the upstream dataset. Because of this limited local signal, we adopted a subsampling strategy that retains all quality-passing Washington sequences, ensuring that no local information is lost. To contextualize Washington sequences without overwhelming the build, we included (1) U.S. non-WA background sequences, (2) broader North American sequences, and (3) light global sampling sufficient to preserve genotype structure. This layered approach allows outbreaks or importations into Washington to be interpreted against the most epidemiologically relevant background while still preserving major global rubella genotypes.

- **Always retain all Washington sequences**
   - Implemented through the `wa_all` query:

      `query: '(country == "USA") & (division == "Washington")'
max_sequences: 10000`

      This guarantees that WA sequences are never dropped due to down-sampling heuristics.

- **Preserve U.S. context without overshadowing WA**
   - U.S. (non-WA) sequences are sampled lightly by **division-year**, with only 5 sequences per group.
   - This provides enough background to distinguish single introductions from broader U.S. circulation.

- **Preserve North America context**
   - Adds additional context from Canada, Mexico, and the Caribbean.
   - Also sampled lightly (5 per country-year) to avoid overwhelming WA focal data.

- **Include limited global context**
   - Global sequences are sampled at 3 per region-year.
   - The goal is not to recreate the global rubella build but to ensure:
      - genotype diversity (1E, 1G, 2B, etc.),
      - placement of WA samples within global lineages,
      - CRS-relevant genotype interpretation.

- **Temporal Filtering**
   Rubella is a slow-evolving virus, and the upstream dataset contains very old sequences (1940s–1980s). Including these in a Washington public-health build:
   - distorts the tree,
   - adds irrelevant historical branches,
   - hides recent introduction patterns.  
   - Therefore, each build applies: `min_date: "2000-01-01"` 
   - The effect of this is that it drops early historical sequences not relevant to current U.S./WA elimination-phase epidemiology while retaining recent genotype diversity and modern CRS-relevant lineages.
   
   This structure mirrors the logic used in the WA measles and mumps builds: retain WA, enrich near neighbors, lightly sample globally.

Both **E1** and **genome** builds use the same subsampling scheme, ensuring that Washington sequences are analyzed within a consistent North American and global genotype context. The builds also keep any WHO reference genotype strains (forced via include lists) regardless of date.

### Root selection: 
Rubella’s deep historical branches and sparse early sampling make fixed rooting unreliable. Midpoint rooting with TreeTime refinement avoids genotype bias and remains consistent with the upstream Nextstrain approach.

### Reference selection:
The build uses WHO-recommended genotype reference sequences and the upstream Nextstrain rubella references for both the full genome and E1 gene. These reference strains anchor divergence estimates and clade definitions across datasets from multiple countries and decades. For the Washington build, this ensures that rare or unusual local sequences can still be evaluated relative to established genetic lineages, which is essential for importation analyses and for supporting elimination verification frameworks.

### Inclusion/Exclusion Criteria: 

- **Required WHO genotype references**
   - The following files explicitly force in essential genotype references:
      `defaults/include_strains_E1.txt`
      `defaults/include_strains_genome.txt`
   - These ensure:
      - baseline representation of all WHO genotypes,
      - consistent tree rooting and topology,
      - correct visual interpretation of WA sample placement.

- **Sequence quality thresholds**
   - These lengths remove partially reported, fragmentary, or low-coverage sequences.
      - Genome: minimum length 7000 bp
      - E1: minimum length 700 bp

- **Known-problematic sequences**
   - Managed through `defaults/dropped_strains.txt`.


## Getting Started
The build supports both E1 and whole-genome workflows and is structured to ensure reproducibility, clarity, and compatibility with the upstream Nextstrain framework.

Some high-level features and capabilities specific to this build include:

- **Washington-focused subsampling with full retention of WA sequences**

   This build is designed to keep all high-quality Washington sequences (genome and E1), ensuring that local epidemiologic signals are never lost during subsampling. This is essential for case investigation, importation assessment, and elimination-related work.

- **North America–anchored contextual sampling**

   The subsampling strategy prioritizes U.S. and North American sequences to provide meaningful regional context without overwhelming the build with oversampled countries (e.g., Japan). This assists in identifying likely sources of importation and understanding regional circulation patterns.

- **Light global genotype representation for interpretability**
   A curated, minimal set of global sequences is included to maintain genotype structure (e.g., 1E, 2B) and preserve interpretability with WHO and CDC rubella classification systems. This allows local results to be understood in the context of global rubella evolution.

- **Parallel whole genome and E1 gene builds**
   Rubella surveillance often relies heavily on E1 sequencing due to global sequencing practices. This build generates both whole-genome and E1 workflows with harmonized filtering, traits, and refinement parameters, allowing users to switch between the two depending on data availability and analytic needs.

- **Built to align with upstream Nextstrain rubella conventions**
   Reference sequences, clade definitions, naming conventions, and ancestral reconstruction settings are consistent with the official Nextstrain rubella build. This ensures comparability between local Washington-focused analyses and global rubella phylogenetics.

- **Reusable configuration designed for other states/jurisdictions**
   The configuration clearly separates Washington-specific elements from generalizable components. Other jurisdictions can adopt this build template simply by editing the subsampling queries and filepaths.


### Data Sources & Inputs
This build relies on publicly available rubella data aggregated via the core Nextstrain rubella workflow:
- **Sequence data**:
  - Rubella virus sequences curated from **GenBank** and made available via Nextstrain’s rubella workflow (downloaded as `data/sequences.fasta.zst`).
- **Metadata**:
  - Associated sample metadata (collection date, country, division, genotype fields, etc.) available via `data/metadata.tsv.zst`.
- **Expected inputs (after download/decompression)**:
  - `data/sequences.fasta` – nucleotide sequences (E1 and genome).  
  - `data/metadata.tsv` – tab-delimited metadata with fields including `accession`, `date`, `country`, `division`, `region`, genotyping annotations, etc.
- **Washington-specific data**:
  - At present, Washington sequences are those already present in the upstream rubella dataset (currently a small number of WA sequences). The configuration is designed so that if additional WA sequences are added upstream, they will be automatically retained.

### Setup & Dependencies
#### Installation
Ensure that you have [Nextstrain](https://docs.nextstrain.org/en/latest/install.html) installed.

To check that Nextstrain is installed:
```
nextstrain check-setup
```

#### Clone the repository:

```
git clone https://github.com/NW-PaGe/rubella.git
cd rubella/phylogenetic
```

## Run the Build

These instructions assume you’re in the rubella/phylogenetic directory.

The Washington-specific configuration is defined in:

- `washington/config_wa.yaml`

- `washington/auspice_config.json`

To run the full WA-focused E1 + genome builds from scratch:

```
nextstrain build . \
  --configfile washington/config_wa.yaml \
  --cores 8 \
  --forceall \
  --rerun-incomplete
```
- `--configfile washington/config_wa.yaml` applies the Washington-focused settings (subsampling, filters, titles, etc.).

- `--forceall` is recommended whenever the config or profiles change to ensure the whole workflow is rebuilt using the updated logic.

- `--rerun-incomplete` restarts any partially finished jobs cleanly.

### Expected Outputs
The file structure of the repository is as follows with `*`" folders denoting folders that are the build's expected outputs.

```
├── *auspice/
│   ├── rubella_E1.json
│   ├── rubella_E1_tip-frequencies.json
│   ├── rubella_genome.json
│   └── rubella_genome_tip-frequencies.json
└── *results/
    ├── E1/
    │   ├── aligned_and_filtered.fasta
    │   ├── tree.nwk
    │   ├── branch_lengths.json
    │   ├── traits.json
    │   ├── nt_muts.json
    │   └── aa_muts.json
    └── genome/
        ├── aligned_and_filtered.fasta
        ├── tree.nwk
        ├── branch_lengths.json
        ├── traits.json
        ├── nt_muts.json
        └── aa_muts.json

```
More details on the file structure of this build can be found here (link to Wiki page that contains contents of  Repository File Structure Overview section).

After successfully running the build there will be two output folders containing the build results.

- `auspice/` folder contains: a .json file
- `results/` folder contains:

auspice/*.json are the files you can:

- load locally with Auspice / nextstrain view
- `results/` contains intermediate phylogenetic outputs that can be reused or interrogated directly if needed.

### Visualize Results

You can visualize the build outputs using Auspice in two ways:

**1. Load `.json` files into auspice.us**

   - Drag and drop any of the build JSON files (for example, `rubella_E1.json` or `rubella_genome.json`) into:

    `https://auspice.us`
    
   This is useful for rapid, local exploration or sharing with collaborators.

**2. View locally using the Nextstrain CLI**

   - You can launch Auspice locally using the following commands:

```
nextstrain view auspice/rubella_E1.json
nextstrain view auspice/rubella_genome.json
```

These commands open an interactive visualization identical to the hosted Nextstrain view.

**Tree Interpretation Resources:**
- For guidance on interpreting time-resolved phylogenies, mutation patterns, ancestral reconstruction, and geographic trait inference, consult:
   - Nextstrain Augur Interpretation Guide: https://docs.nextstrain.org/projects/augur/en/stable/interpretation/
   - Auspice Visualization Documentation: https://docs.nextstrain.org/projects/auspice/en/stable/

## Customization for Local Adaptation
This build is intended to be a reusable pattern for other jurisdictions (states, provinces, countries) that wish to create their own locally focused rubella builds.

To adapt:

1. **Copy the repository:**
   - Fork or clone and adjust under a new GitHub org or project.

2. **Update subsampling logic in `washington/config_wa.yaml`:**

   - Modify queries in `subsampling`: to match your region:

   - Replace `(division == "Washington")` with your state/region.

   - Adjust the `usa_context`, `na_context`, and `global_refs` strata to reflect the geographic relationships you care about (e.g., continental context, neighboring countries).

3. **Rename the profile folder:**

   - `washington/` can be cloned and renamed to e.g. `my_state/` and referenced via `--configfile my_state/config_<name>.yaml`.

4. **Adjust titles & text:**

   - Update Auspice titles in both:
      - `config_*.yaml` (build titles), and
      - `auspice_config.json` (overall dataset title, colorings, metadata columns, etc.).

5. **Integrate local/private sequences (optional):**
   - If your jurisdiction has additional sequences not yet in the upstream rubella dataset:
      - extend the workflow to ingest and merge them into `data/sequences.fasta` and `data/metadata.tsv` prior to running the phylogenetic pipeline, or
      - host a separate ingest workflow that outputs harmonized inputs in the same format.

*Since rubella is tied so closely to CRS surveillance, please mote that CRS investigations often rely on maternal–infant paired sequences. Local jurisdictions integrating private CRS case data may require additional ingest scripts to merge these sequences with the global dataset.*
## Contributing
For questions, feature requests, or scientific discussion related to this build:

- Open a GitHub Issue in this repository to report bugs or request enhancements.
- Use your program’s standard communication channels (e.g., Discussions, internal documentation) if you are adapting this build inside another organization.

Contributions in the form of:
- improvements to subsampling logic,
- better integration of local/private data,
- updated references and genotyping schemes,
are all welcome via pull request.
## License
This project is licensed under a modified GPL-3.0 License.
You may use, modify, and distribute this work, but commercial use is strictly prohibited without prior written permission.

## Acknowledgements

We gratefully acknowledge:

- The Nextstrain team and the maintainers of the core rubella workflow.
- Researchers and public health labs worldwide that share rubella sequences via GenBank and other repositories.
- Colleagues in the Washington Department of Health Molecular Epidemiology team and partner laboratories who contribute to rubella and CRS surveillance.
- Collaborators at local, national, and international institutions who support the development, review, and use of these Washington-focused builds.
This work would not be possible without open data sharing and cross-jurisdictional collaboration.
<!-- Repository File Structure Overview [**Move contents of this section to Wiki**]
(This section outlines the high-level file structure of the repo to help folks navigate the repo. If the build follows the pathogen template repo feel free to make this section brief and link to the pathogen template repo resource)*

Example text below:

This Nextstrain build follows the structure detailed in the [Pathogen Repo Guide](https://github.com/nextstrain/pathogen-repo-guide).
Mainly, this build contains [number] workflows for the analysis of [pathogen] virus data:
- ingest/ [link to ingest workflow] Download data from [source], clean, format, curate it, and assign clades.
- phylogenetic/ [link to phylogenetic workflow] Subsample data and make phylogenetic trees for use in nextstrain.

OR
The file structure of the repository is as follows with `*`" folders denoting folders that are the build's expected outputs.

```
.
├── README.md
├── Snakefile
├── auspice*
├── clade-labeling
├── config
├── new_data
├── results*
└── scripts
```

- `Snakefile`: Snakefile description
- `config/`: contains what
- `new_data/`: contains What
- `scripts/`: contains what
- `clade-labeling`: contains what
-->
