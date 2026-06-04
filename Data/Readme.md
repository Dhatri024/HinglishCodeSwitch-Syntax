# Dataset Access

## Overview

The HinglishCodeSwitch-Syntax (HCSS) dataset is hosted on Hugging Face due to its large size and is not stored directly in this GitHub repository.

This repository contains:

* Documentation
* Data schema definitions
* Dataset generation methodology
* Research notebooks
* Visualization scripts
* Data processing utilities

The complete dataset can be downloaded from the Hugging Face Dataset Hub.

---

## Download Dataset

**Hugging Face Dataset Repository**

```text
https://huggingface.co/datasets/dhatri-02/HCSS-v1.4.2-MLF
```

---

## Dataset Contents

The Hugging Face repository contains:

```text
data/
├── raw/
│   └── HinglishCodeSwitch_Syntax_v1_raw.parquet
│
└── splits/
    ├── hinglish_codeswitch_v1_train.parquet
    ├── hinglish_codeswitch_v1_val.parquet
    └── hinglish_codeswitch_v1_test.parquet
```

---

## Dataset Statistics

| Property      | Value                                   |
| ------------- | --------------------------------------- |
| Dataset Name  | HinglishCodeSwitch-Syntax (HCSS)        |
| Version       | v1.4.2-MLF                              |
| Total Records | 1,000,000                               |
| Features      | 45                                      |
| Format        | Apache Parquet                          |
| Language      | Hinglish (Hindi-English Code-Switching) |
| Encoding      | UTF-8                                   |
| License       | CC BY 4.0                               |

---

## Recommended Download

Most users should download the predefined splits:

```text
hinglish_codeswitch_v1_train.parquet
hinglish_codeswitch_v1_val.parquet
hinglish_codeswitch_v1_test.parquet
```

The raw dataset is provided for researchers who wish to create custom train/validation/test partitions.

---

## Documentation

Additional documentation is available in the repository:

```text
docs/
├── data_dictionary.md
├── methodology.md
├── limitations.md
└── dataset_card.md
```

---

## Citation

If you use this dataset in research, projects, or publications, please cite the dataset repository and provide appropriate attribution.

---

## License

This dataset is distributed under the Creative Commons Attribution 4.0 International (CC BY 4.0) License.
