# MultiMETu: Multimodal Metaphor Understanding Dataset

**MultiMETu** is a dataset for multimodal metaphor understanding and generation in multimodal large language models (MLLMs). It contains **6,450 annotated image–text samples** collected from public advertising and web sources. Each sample pairs an image with the text appearing in it and provides fine-grained metaphor annotations: the target domain, an inappropriate target, appropriate and inappropriate source domains, primary and secondary source-domain attributes, and a natural-language metaphor description.

The dataset accompanies the paper:

> Senqi Yang, Dongyu Zhang, Zihang Du, Haojia Li, Guiru Wang, Qi Wu, Wanqiu Liao, Zeqi Hao, Wei Guo, Mingshuo Pan, Hongfei Lin. **Multimodal Metaphor Understanding and Generation in MLLMs: A Dataset and Reasoning Framework.** *Pattern Recognition*, 2026, 178: 113369. DOI: [10.1016/j.patcog.2026.113369](https://doi.org/10.1016/j.patcog.2026.113369)

## Data access

The complete dataset — all 6,450 images plus the annotation files, about **674 MB** — is publicly available on Hugging Face:

**<https://huggingface.co/datasets/senqiY/MultiMETu>**

This repository keeps a small demonstration subset of the images (473 JPG files) and the full `final.json` annotation file. For the complete image set and the full annotation table (`final.xlsx`), use the Hugging Face release.

### Repository layout on Hugging Face

```
MultiMETu/
├── image/
│   ├── 0.jpg
│   └── ...                # 6,450 images
├── final.json             # 6,450 records
├── final.xlsx             # 6,450 rows, 11 columns
└── README.md
```

### Download

```bash
huggingface-cli download senqiY/MultiMETu --repo-type dataset --local-dir MultiMETu
```

Or in Python:

```python
from huggingface_hub import snapshot_download

path = snapshot_download("senqiY/MultiMETu", repo_type="dataset")
```

## Annotation fields

Images are stored as `image/{ID}.jpg`. `final.json` is the annotation file used by the released benchmark; `final.xlsx` is the full annotation table and additionally contains the secondary source-domain attributes.

| `final.json` | `final.xlsx` | Description |
|---|---|---|
| `ID` | `ID` | Sample identifier; the image is `image/{ID}.jpg`. |
| `Text_in_the_image` | `Text_in_the_image` | Text appearing in the image. |
| `Target` | `Target` | Target domain of the metaphor. |
| `Target_inapt` | `Target_inapt` | Inappropriate target-domain option. |
| `Source_apt` | `Source_apt` | Appropriate source domain. |
| `Source_inapt` | `Source_inapt` | Inappropriate source domain. |
| `source_attribute_apt1` | `ground_apt1` | Primary attribute of the appropriate source domain. |
| — | `ground_apt2` | Secondary attribute of the appropriate source domain. |
| `source_attribute_inapt1` | `ground_inapt1` | Primary attribute of the inappropriate source domain. |
| — | `ground_inapt2` | Secondary attribute of the inappropriate source domain. |
| `metaphor_describe` | `paraphrase` | Natural-language description of the metaphor. |

Some attribute fields contain multiple comma-separated values; empty cells indicate that the corresponding secondary attribute was not annotated.

## Loading example

```python
import json
from pathlib import Path

from huggingface_hub import snapshot_download

root = Path(snapshot_download("senqiY/MultiMETu", repo_type="dataset"))
records = json.loads((root / "final.json").read_text(encoding="utf-8"))
sample = records[0]

image_path = root / "image" / f"{sample['ID']}.jpg"
print(image_path)
print(sample["Target"], "->", sample["Source_apt"])
print(sample["source_attribute_apt1"])
print(sample["metaphor_describe"])
```

## Licensing

- **Annotations and metadata** (the JSON/XLSX files) are released under the **Creative Commons Attribution-NonCommercial 4.0 International License (CC BY-NC 4.0)**: <https://creativecommons.org/licenses/by-nc/4.0/>.
- **Images** were collected from public advertising and web sources and remain the property of their respective rights holders. They are provided **solely for non-commercial academic research**. Users are responsible for complying with the applicable rights and terms of the original sources and for obtaining any additional permissions required for other uses.

If you believe that any content in this dataset infringes your rights, please contact the authors and the material will be removed.

## Citation

```bibtex
@article{yang2026multimodal,
  title   = {Multimodal Metaphor Understanding and Generation in MLLMs: A Dataset and Reasoning Framework},
  author  = {Yang, Senqi and Zhang, Dongyu and Du, Zihang and Li, Haojia and Wang, Guiru and Wu, Qi and Liao, Wanqiu and Hao, Zeqi and Guo, Wei and Pan, Mingshuo and Lin, Hongfei},
  journal = {Pattern Recognition},
  volume  = {178},
  pages   = {113369},
  year    = {2026},
  doi     = {10.1016/j.patcog.2026.113369}
}
```
