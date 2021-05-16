# CombSAFE

## About
This repository contains the implementation of the *Combinatorial and Semantic Analysis of Functional Elements* (CombSAFE) presented in: ```Leone M., Galeota E., Ceri S., Masseroli M., and Pellizzola M. "Identification, semantic annotation and comparison of regulatory functional element combinations in multiple biological conditions", 2021```. It is a flexible computational method to identify combinations of static and dynamic functional elements genome-wide, and how they change across semantically annotated biological conditions. 

Given as input a set of ChIP-seq dataset samples and the list of functional elements to be considered, the ```CombSAFE``` pipeline allows:
- considering histone modifications, transcription factors and any other type of dynamic and static genomic features (e.g., CpG islands, partially methylated domains, transposable elements, etc.)
- relying on public repositories to retrieve the considered static functional elements and identify from the input dataset metadata the biological conditions in which the dynamic functional elements of interest were charted
- leveraging natural language processing techniques and biomedical ontologies to complement the identified conditions with semantic annotations about tissue and disease types
- identifying combinations of static and dynamic functional elements throughout the genome in the corresponding omics data using hidden Markov models 
- focusing on specific genomic regions, applying clustering to explore how significant combinations of the functional elements compare among semantically annoted cell types and desease/healthy conditions 
- performing functional enrichment analyses based on the genes found in genomic regions with similar combinations of functional elements.

## Cookbook
In the following, we show how to call the functions implemented to easily perform the different steps of our ```CombSAFE``` computational method, providing example resuls for some of them. 

### Load input files
```combsafe.import_path(filepath)```<br/>
Load the input path for the analysis<br/>

Parameters: 
- ***filepath***: path object or file-like object 

Example:
```python
>> import_path("./path_to_files/")
```

### Generate semantic annotations
```combsafe.generate_semantic_df(separator, encode_convert)```<br/>
Generate semantic annotations about tissue and disease types from the input dataset<br/>

Parameters: 
- ***sep***: str, default '\t'
  - Delimiter to use for the input .txt file
- ***encode_convert***: bool, default False
  - If true, encode IDs are searched to be converted to GSM

Returns: 
  - ***DataFrame*** or ***TextParser***
    - A comma-separated values (csv) file is returned as two-dimensional data structure with labeled axes

Example:
```python
>> semantic_df = generate_semantic_df(sep="\t", encode_convert=True)
```

### Data analysis
```combsafe.plot_factor_freq(dataframe, n)```<br/>
Vertical barplot of the factor frequency in the input dataset<br/>

Parameters: 
- ***dataframe***: str, default '\t'
  - Dataframe of semantic annotations
- ***n***: int
  - Number of Factor to diplay in the barplot

Example:
```python
>> plot_factor_freq(semantic_df, 30)```
```

![alt text](https://drive.google.com/uc?export=download&id=1WyFjK1eYM9nSbMKLht0dXp6ouscZ381P) <br/>



```combsafe.generate_fixed_factor_pool(dataframe, factor_list, number_of_semantic_annotation)``` <br/>
Table containg lists of factors according to the selected parameters

Parameters: 
- ***dataframe***: dataframe
  - Dataframe of semantic annotations
- ***factor_list***: list
  - List of factor to include in the analysis
- ***number_of_semantic_annotation***: int
  - Number of semantic annotations to include in the analysis

Example:
```python
>> combsafe.generate_fixed_factor_pool(semantic_df, ["CTCF", "MYC"], 5)
```

![alt text](https://drive.google.com/uc?export=download&id=1Qc4W9vm2ekev_P13-56akRNpK_oY92BQ)

```combsafe.get_semantic_annotation_list(semantic_df, ["CTCF", "MYC", "POLR2A", "H3K4me3", "H3K27me3"])```

![alt text](https://drive.google.com/uc?export=download&id=1llQnJyeJku6evCgDaOymWuiIgCE5dYXO)


### Select features and combine samples

```combsafe.run_gmql(["CTCF", "MYC", "POLR2A", "H3K4me3", "H3K27me3"])```

### [Optional] Add custom tracks

```combsafe.add_custom_tracks(track_lable, path_to_custom_track, index)```

### Identify functional states 

```combsafe.identify_chromatin_states(number_of_states, n_core)```<br/>
```combsafe.show_emission_graph(custom_palette=pool_3_palette)```
![alt text](https://drive.google.com/uc?export=download&id=1Kk_vOm5wz_ski-fLvTxB48dhhu9TXcNY)

### Genome-wide analysis

```reducted_df = genome_reduction(full_df)```<br/>
```data_driven_heatmap(reducted_df)```
![alt text](https://drive.google.com/uc?export=download&id=1jbyS_WY54SfJtCWQhw9tpiYW8vC2QJ_Q)


```gene_ontology_enrichment_analysis(clustered_heatmap, reducted_df, sig_cut_off= 0.05)```

### Single-gene analysis

...

### [Optional] Semantic analysis

...
