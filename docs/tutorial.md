---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.16.7
kernelspec:
  display_name: Python 3 (ipykernel)
  language: python
  name: python3
---

# 튜토리얼

이 노트북은 CellSAM 사용에 대해 사용자를 안내합니다. CellSAM에 대한 자세한 내용은 사전 인쇄: <https://www.biorxiv.org/content/10.1101/2023.11.17.567630v3>에서 확인할 수 있습니다.

생물학자를 위해, 우리는 {func}`~cellSAM.cellsam_pipeline.cellsam_pipeline`을 사용하는 것을 권장하며, 이 함수는 이미지를 받아 자동으로 가중치를 다운로드하고 마스크를 반환합니다. `cellsam_pipeline`에는 postprocessing, 정규화, 대조 향상 등의 추가 기능이 있지만, 종종 즉시 사용할 수 있습니다. 머신 러닝 실무자이거나 더 특수한 용도의 사용자인 경우, `get_model`을 통해 모델과 가중치에 직접 액세스할 수 있습니다.

추가적인 도움이나 지원이 필요한 경우 [연락][gh-issue]를 주시기 바랍니다.

[gh-issue]: https://github.com/vanvalenlab/cellSAM/issues

```{code-cell} ipython3
import imageio.v3 as iio
import numpy as np
import scipy as sp
import matplotlib.pyplot as plt
import matplotlib.patches as patches

from cellSAM import cellsam_pipeline, get_model
from cellSAM.utils import format_image_shape, normalize_image
```

## CellSAM을 사용한 모든 세포 찾기

이미지 전체의 모든 마스크를 예측하는 데 CellSAM을 직접 사용하는 방법을 보여줍니다. 샘플 이미지를 로드하여 추론 파이프라인을 통과시켜 보겠습니다. 바운딩 박스가 제공되지 않으면 `CellFinder` 모듈이 모든 세포에 대해 바운딩 박스를 자동으로 찾습니다. 추론 파이프라인은 GPU를 찾으면 사용하고, 그렇지 않으면 CPU로 폴백합니다.

```{note}
CellSAM은 대규모 세포 수가 있는 경우 CPU에서도 작동하겠지만 상당히 느립니다.
```

```{code-cell} ipython3
# 샘플 데이터 로드 및 패딩 트림
img = iio.imread("../sample_imgs/YeaZ.png")[8:-7, 8:-7]

# 추론 파이프라인 실행
mask = cellsam_pipeline(
    img, use_wsi=False, low_contrast_enhancement=False, gauge_cell_size=False
)

# 결과 시각화
plt.imshow(mask);
```

# CellSAM 프롬프트 지정

```{note}
interactive segmentation을 위해 {doc}`napari` 플러그인을 확인해 보세요.
```

특정 세포를 라벨링하고 싶은 경우, 이는 CellSAM를 새로운 Morphologies나 세포 유형을 위해 라벨링을 가속화하는 자연스러운 결과입니다. 특정 상자를 지정하고 그 세포만 세그먼테이션하는 방법을 보여드리겠습니다. 모델을 직접 사용하는 방법을 사용합니다.

```{code-cell} ipython3
# 우리가 세그먼테이션하고자 하는 세포입니다!
box = [290, 365, 60, 38] # x, y, w, h

rect = patches.Rectangle(
    (box[0], box[1]), box[2], box[3], linewidth=1, edgecolor='r', facecolor='none'
)

plt.figure(figsize=(5, 5))
plt.imshow(img, cmap='gray')
plt.gca().add_patch(rect);
```

```{code-cell} ipython3
# 여기서는 원하는 세포를 예측합니다!
predictor = get_model()

# 바운딩 박스를 모델 예측 함수에 전달합니다
x1, y1, w, h = box

# 바운딩 박스 프롬프트는 (x1, x2, y1, y2) 형식이어야 하며, (x1, y1)은 좌측 하단 코너이고 (x2, y2)는 우측 상단 코너입니다
x2, y2 = x1 + w, y1 + h

# 이미지 전처리
im = format_image_shape(img)
im = normalize_image(im)
im = np.transpose(im, (2, 0, 1))
im = np.expand_dims(im, axis=0)
pred_mask, *_ = predictor.predict(im, boxes_per_heatmap=[[[x1, y1, x2, y2]]])
```

이제 예측된 마스크를 시각화해 보겠습니다. 마스크를 이미지 위에 중첩하여 더 명확하게 볼 수 있습니다.

```{code-cell}
# 마스크 가장자리를 표시하기 위해 several iterations 사용
dilated_mask = sp.ndimage.binary_dilation(pred_mask > 0, iterations=5)
edges = (dilated_mask > pred_mask).astype(np.uint8)

# 이미지 표시
plt.imshow(img, cmap="gray")

# 마스크 가장표 표시
plt.imshow(255 * edges, cmap="Reds", alpha=edges);
```

그리고 그게 다입니다!

CellSAM에 대한 추가 정보는 자유롭게 문의하거나 공식 배포처인 <https://huggingface.co/vanvalenlab/cellsam>를 확인하세요.