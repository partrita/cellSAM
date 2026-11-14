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

# Napari 플러그인

CellSAM는 로컬, 상호작용 이미지 세그먼테이션을 돕기 위해 ``napari`` 플러그인을 포함합니다.

## 설치

플러그인은 ``napari``가 설치되어 있어야 합니다. ``cellSAM``을 설치할 때 선택적 Napari 의존성을 설치하는 가장 간단한 방법은 다음과 같습니다.

소스에서 로컬로 설치하는 경우:

```bash
uv sync --extra napari
```

아니요, 모든 필요한 의존성 없이 설치하려면:

```bash
uv sync --extra napari
```

```{note}
선택적 의존성은 기본적으로 PyQt6을 포함합니다. Qt 바인딩을 더 세밀하게 제어하려면 의존성을 수동으로 설치하는 것을 고려해 보세요, 예를 들어 `uv add`를 사용하여:

```bash
uv add napari pyside2 magicgui
```

아니요, 단순히 실행하려면:

```bash
uv sync --extra napari
```

Napari 문서 [napari documentation][napari-install]에 Qt 백엔드 선택에 관한 자세한 내용이 있습니다.

[napari-install]: https://napari.org/stable/tutorials/fundamentals/installation.html#choosing-a-different-qt-backend
```

## 플러그인 실행

Napari 창을 열고 cellSAM 플러그인을 사용하려면:

```bash
cellam napari
```

### 이미지 로드

화면 오른쪽 상단의 `File` 메뉴를 선택하고 `Open File(s)`를 선택하여 열기 프롬프트를 표시합니다. 여기서 `sample_imgs/` 디렉토리에서 `tissuenet.png`와 같은 열기를 선택할 수 있습니다:

![napari-open](_static/napari_img_load_menu.png)

```{note}
플러그인은 현재 `.png`와 같은 일반적인 이미지 형식을 지원합니다. 추가 형식 지원이 필요한 경우 [기능 요청]![gh-issues]을 만들어 주세요!
```

[gh-issues]: https://github.com/vanvalenlab/cellSAM/issues

### 상호작용 세그먼테이션

플러그인은 CellSAM 모델이 바운딩 박스 내의 세포를 세그먼테이션하도록 지정하는 바운딩 박스를 사용자에게 허용합니다.

```{note}
플러그인은 바운딩 박스당 하나의 세포가 있다고 가정합니다. 이미지 영역 전체의 많은 세포에 대한 세그먼테이션을 수행하려면 {func}`~cellSAM.cellsam_pipeline.cellsam_pipeline` 라이브러리 인터페이스를 사용해 주세요.
```

바운딩 박스는 다음과 같이 그립니다:

1. 왼쪽 패널에서 "Bounding Boxes" 레이어를 선택합니다.
2. 도구 패널의 왼쪽 상단에서 "Add Rectangle" 옵션을 선택합니다:

   ![napari-rect-select](_static/napari_rect_select.png)

3. 이제 세그먼테이션할 세포에 해당하는 영역을 선택합니다:

   ![napari-select-cell](_static/napari_select_cell.png)

4. 마지막으로 오른쪽 패널의 "Confirm Annot." 버튼을 누릅니다. 이는 강조 표시된 세포에 대한 세그먼테이션 마스크를 모델에 예측하도록 프롬프트합니다.

   ![napari-single-cell-segmented](_static/napari_single_cell_segmented.png)

이 과정을 여러 번 반복하여 여러 세포를 세그먼테이션할 수 있습니다. 세그먼테이션된 세포는 메인 캔버스에 표시되며 `Segmentation Overlay` 레이어에 캡처됩니다.

### 결과 지우기

표준 Napari 라벨링 도구를 사용하여 개별 예측을 제거할 수 있습니다. `Segmentation Overlay` 레이어에서 가능합니다.

`Clear mask` 오른쪽 패널의 버튼은 현재 세션에서 모든 예측을 제거하는 데 사용됩니다(즉, 세그먼테이션 마스크를 지움). `Reset` 버튼은 현재 예측과 기본 이미지를 모두 제거하며, 기존 세션에서 새 이미지를 열어야 할 경우에만 사용해야 합니다.

## 전체 이미지 세그먼테이션

Napari 플러그인은 오른쪽 패널에 `Segment All` 버튼을 제공하여 이미지 전체의 세포를 자동으로 세그먼테이션할 수 있습니다.

```{caution}
Napari 플러그인은 주로 상호작용 세그먼테이션을 목적으로 합니다. 다음 조건에 해당하는 경우 `Segment All` 버튼은 권장되지 않습니다.

 - 이미지가 2k x 2k 픽셀보다 큰 경우
 - 로컬에 GPU가 사용되지 않는 경우
```

{func}`~cellSAM.cellsam_pipeline.cellsam_pipeline` 라이브러리 인터페이스를 전체 이미지 세그먼테이션에 사용하는 것이 권장됩니다. 시각화와 결합한 인ference 파이프라인 사용 예시는 {ref}`example_gallery`을 참조하세요.
```