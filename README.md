# Oxford-IIIT Pets Dataset for CasCAM

This repository contains the Oxford-IIIT Pet dataset prepared for CasCAM (Cascaded Class Activation Mapping) experiments.

## Dataset Overview

- **Total size**: 644 MB
- **Total images**: 14,780 (7,390 × 2 versions)
- **Annotations**: 11,080 files
- **Classes**: 37 pet breeds (25 dogs, 12 cats)

## Directory Structure

```
oxford-pets-cascam/
├── annotations/          # 45 MB - Segmentation annotations
│   ├── trimaps/         # Trimap segmentation masks
│   └── xmls/            # Bounding box annotations (PASCAL VOC format)
├── original/            # 299 MB - Original clean images
│   └── *.jpg            # 7,390 images
└── with_artifact/       # 301 MB - Images with synthetic artifacts
    └── *.jpg            # 7,390 images
```

## Data Attribution

### Original Dataset
- **Name**: Oxford-IIIT Pet Dataset
- **Source**: [Visual Geometry Group, University of Oxford](https://www.robots.ox.ac.uk/~vgg/data/pets/)
- **Authors**: O. M. Parkhi, A. Vedaldi, A. Zisserman, C. V. Jawahar
- **Citation**:
  ```bibtex
  @inproceedings{parkhi12a,
    title={Cats and Dogs},
    author={Parkhi, O. M. and Vedaldi, A. and Zisserman, A. and Jawahar, C. V.},
    booktitle={IEEE Conference on Computer Vision and Pattern Recognition},
    year={2012}
  }
  ```

### Annotations
- **Segmentation masks (trimaps)**: From Oxford-IIIT Pet Dataset
- **Bounding boxes (XML)**: Generated from trimap annotations
- **Format**: PASCAL VOC XML format

### License
This dataset follows the original Oxford-IIIT Pet Dataset license terms. Please refer to the [original dataset website](https://www.robots.ox.ac.uk/~vgg/data/pets/) for licensing information.

## Modifications for CasCAM

### Original Images
The `original/` directory contains unmodified images from the Oxford-IIIT Pet Dataset, resized and preprocessed for CasCAM experiments.

### Images with Artifacts
The `with_artifact/` directory contains images with synthetically added visual artifacts (noise, occlusions, etc.) for evaluating the robustness of Class Activation Mapping methods.

**Artifact types**:
- Random noise patterns
- Geometric occlusions
- Color perturbations
- Other visual distractors

These modifications were created specifically for CasCAM research to test the method's ability to focus on relevant object features while ignoring irrelevant artifacts.

## Usage

### Clone this repository
```bash
git clone https://github.com/your-username/oxford-pets-cascam.git
```

### Use with CasCAM
This dataset is designed to work with the [CasCAM repository](https://github.com/your-username/CasCAM):

```bash
# Clone CasCAM repository
git clone https://github.com/your-username/CasCAM.git
cd CasCAM

# Clone data repository
git clone https://github.com/your-username/oxford-pets-cascam.git data

# Run experiments
python run.py --data_path ./data/with_artifact/
```

## Dataset Statistics

| Category | Count |
|----------|-------|
| Total breeds | 37 |
| Dog breeds | 25 |
| Cat breeds | 12 |
| Images per category (avg) | ~200 |
| Image format | JPEG |
| Average image size | ~40 KB |

## Annotation Format

### Trimaps
- Format: PNG images
- Values:
  - 1: Foreground (pet)
  - 2: Boundary
  - 3: Background

### XML Annotations
- Format: PASCAL VOC XML
- Contains:
  - Bounding box coordinates (xmin, ymin, xmax, ymax)
  - Class labels
  - Image dimensions

## Related Research

This dataset is used in the following research:

```bibtex
@misc{cascam2025,
  title={CasCAM: Cascaded Class Activation Mapping for Enhanced Model Interpretability},
  author={Seoyeon Choi and Hayoung Kim and Guebin Choi},
  year={2025},
  note={Research implementation},
  institution={Jeonbuk National University, KT Corporation}
}
```

## Contact

For questions about this dataset preparation or CasCAM experiments, please open an issue in the [CasCAM repository](https://github.com/your-username/CasCAM).

## Acknowledgments

We thank the Visual Geometry Group at the University of Oxford for creating and sharing the Oxford-IIIT Pet Dataset.