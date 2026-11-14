# CellSAM: A Foundation Model for Cell Segmentation

**NOTE** Be sure to update to the [latest version of the model][pinned-issue]

[pinned-issue]: https://github.com/vanvalenlab/cellSAM/issues/90

## 설명
이 리포지토리는 CellSAM에 대한 추론 코드를 제공합니다. CellSAM은 [사전 인쇄](https://www.biorxiv.org/content/10.1101/2023.11.17.567630v3)에 대해 더 자세히 설명되어 있으며, [https://huggingface.co/vanvalenlab/cellsam](https://huggingface.co/vanvalenlab/cellsam)에서 공개적으로 배포됩니다. CellSAM는 박테리아, 조직, 효모, 세포 배양 등 다양한 세포 표적 및 영상 모달리티(밝은 빛, 형광, 위상 등)에서 최신 성능을 달성합니다. 지원/질문이 있으면 [연락](mailto:ulisrael@caltech.edu) 주세요! 모델 가중치는 Hugging Face Hub에서 이용할 수 있습니다. 전체 평가 데이터셋은 별도 라이선스하에 제공되며 적절한 경로를 통해 얻을 수 있습니다.

## 시작하기
CellSAM를 시작하는 가장 쉬운 방법은 이 리포지토리를 클론하고 `uv`를 사용하는 것입니다:

```bash
git clone https://github.com/vanvalenlab/cellSAM.git
cd cellSAM
uv sync
```

로컬 개발의 경우, 이 리포지토리는 [uv](https://docs.astral.sh/uv/)를 사용하여 Python 환경 및 의존성을 관리합니다. uv를 설치한 다음 다음과 같이 실행하세요:

```bash
uv sync
```

선택 의존성을 설치해야 하는 경우:

```bash
# Napari 주석 지원
uv sync --extra napari

# 원격 데이터 액세스
uv sync --extra remote-data

# 개발 및 문서화 의존성
uv sync --group dev --group docs
```

`uv run`을 사용하여 관리된 환경에서 명령을 실행하세요:

```bash
uv run python your_script.py
uv run pytest
```

의존성을 변경하면 `uv add`로 프로젝트 메타데이터를 업데이트(또는 `pyproject.toml`을 편집)하고 `uv lock`로 lockfile을 재생성하세요. contributor가 재현 가능한 환경을 사용할 수 있도록 `pyproject.toml`과 생성된 `uv.lock`을 모두 커밋하세요.

CellSAM는 `python>=3.10`을 요구하지만, 그 외에는 순수 PyTorch를 사용합니다. 샘플 이미지가 이 리포지토리에 포함되어 있습니다. 세그먼테이션은 다음과 같이 수행할 수 있습니다:

```python
import numpy as np
from cellSAM import segment_cellular_image

img = np.load("sample_imgs/yeaz.npy")
mask, _, _ = segment_cellular_image(img, device="cuda")
```

더 자세한 내용은 [튜토리얼](https://vanvalenlab.github.io/cellSAM/tutorial)을 참조하세요.

### Napari 패키지
CellSAM에는 기본 Napari 주석 패키지가 포함되어 있습니다. Napari 추가 의존성을 설치하려면 `napari` 선택적 의존성 그룹을 사용하세요:

```bash
uv sync --extra napari
```

Napari 앱을 시작하려면 `uv run cellsam napari`를 실행하세요. Napari 플러그인 문서에 대한 자세한 내용은 [https://vanvalenlab.github.io/cellSAM/napari.html](https://vanvalenlab.github.io/cellSAM/napari.html)를 확인하세요.

## 인용
CellSAM를 사용하는 경우 참고해 주세요.

```
@article{israel2023foundation,
  title={A Foundation Model for Cell Segmentation},
  author={Israel, Uriah and Marks, Markus and Dilip, Rohit and Li, Qilin and Schwartz, Morgan and Pradhan, Elora and Pao, Edward and Li, Shenyi and Pearson-Goulart, Alexander and Perona, Pietro and others},
  journal={bioRxiv},
  publisher={Cold Spring Harbor Laboratory Preprints},
  doi = {10.1101/2023.11.17.567630},
}
```