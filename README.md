# Oxford-IIIT Pets Dataset for CasCAM

Oxford-IIIT Pets for CasCAM provides three synchronized views of every image pair used in our artifact-robustness experiments:

- `original/` — the clean Oxford-IIIT Pet images (7,390 files)
- `with_artifact/` — the same images with synthetic occlusion/noise artifacts applied
- `artifact_boxes/` — rectangular binary masks that highlight the artifact footprint for each perturbed image
- `annotations/` — the original trimaps, VOC-style XMLs, and split files distributed with Oxford-IIIT Pets

The repository is ready to drop into the CasCAM codebase or any project that needs aligned clean, perturbed, and artifact-region supervision.

## Directory layout
```
oxford-pets-cascam/
├── annotations/
│   ├── trimaps/               # foreground/boundary/background trimap PNGs
│   ├── xmls/                  # VOC-style bounding boxes from the original dataset
│   ├── list.txt               # full image listing with class ids
│   ├── trainval.txt           # official train/val split
│   └── test.txt               # official test split
├── artifact_boxes/
│   ├── *.png                  # 512×512 binary masks (0 background / 255 artifact box)
│   └── artifact_metadata.csv  # per-image thresholds and bounding boxes
├── original/
│   └── *.jpg                  # clean images
└── with_artifact/
    └── *.jpg                  # images with injected artifacts
```

## Artifact bounding boxes
The synthetic artifacts were detected by contrasting `original/` and `with_artifact/` images with an adaptive per-image threshold. Each mask in `artifact_boxes/` contains the tight bounding rectangle that encloses the detected artifact region; pixel values are either 0 or 255 to keep the files compact and easy to visualize.

`artifact_metadata.csv` summarises the detection outcome for every image. Columns:

| column | description |
| ------ | ----------- |
| `image` | filename shared by `original/` and `with_artifact/` |
| `threshold` | normalized difference threshold chosen for the image |
| `artifact_pixels` | number of pixels inside the rectangular mask |
| `artifact_ratio` | artifact_pixels divided by the total image area |
| `bbox_xmin`, `bbox_ymin`, `bbox_xmax`, `bbox_ymax` | bounding rectangle coordinates (0-indexed) |

You can treat these masks as weak supervision for bounding-box based training, or expand the rectangle to a soft mask if needed.

## Using the dataset with CasCAM
```bash
# Clone the CasCAM codebase
git clone https://github.com/guebin/CasCAM.git
cd CasCAM

# Place this repository in the expected data location
git clone https://github.com/guebin/oxford-pets-cascam.git data

# Example: train with artifact-augmented images
python run.py --data_path ./data/with_artifact/

# Example: evaluate with artifact bounding boxes
python tools/eval.py \
  --original ./data/original/ \
  --perturbed ./data/with_artifact/ \
  --artifact-masks ./data/artifact_boxes/
```

To regenerate the rectangular masks after updating the artifact pipeline, run the `run251018-extract_artifacts.py` script from the 2025-HY-CasCAM repository with `--mask-shape bbox` and point it at the `original` and `with_artifact` folders.

## Dataset statistics
- 37 total breeds (12 cats, 25 dogs)
- ~200 images per breed, JPEG format
- Image resolution is 512×512 after preprocessing
- Artifact masks occupy 1–15 % of the image area depending on the injected pattern

## Attribution and licence
- **Original dataset**: [Oxford-IIIT Pet Dataset](https://www.robots.ox.ac.uk/~vgg/data/pets/) by O. M. Parkhi, A. Vedaldi, A. Zisserman, and C. V. Jawahar
- **Original annotations**: Trimaps, VOC XMLs, and splits copied verbatim from the official release
- **CasCAM modifications**: synthetic artifacts and bounding-box masks created for interpretability research

This repository is provided for research purposes only. Check the Oxford-IIIT Pet terms of use before deploying the images or derivatives in commercial products.
