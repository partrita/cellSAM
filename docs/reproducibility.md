# 재현성

CellSAM 라이브러리 외에도 게재된 결과를 재현하는 데 도움이 추가적인 코드가 포함된 [`paper_evaluation`][gh-eval] 디렉토리를 확인하세요.

[gh-eval]: https://github.com/vanvalenlab/cellSAM/tree/master/paper_evaluation

재현성을 위한 추가 리소스에는 사전 학습된 모델 가중치와 평가 데이터셋이 포함됩니다. 모든 필수 구성요소는 {doc}`API-key`에서 세부 정보를 확인할 수 있습니다.

## 설정

새로운 (빈) 가상 환경에서 부모 디렉토리에서 cellSAM을 설치하세요.

````{admonition} 예: 환경 생성
:class: tip dropdown

사용자는 가장 편한 환경 관리 시스템을 사용하시기를 권장합니다 (`uv`, `pixi`, `conda/mamba` 등)

아직 익숙하지 않으시다면, Python의 [내장 환경 관리 모듈][python-venv]는 간단하고 널리 사용되는 옵션입니다. 예를 들어, 새로운 환경을 생성하고 진입하려면 다음과 같이 하세요:

```bash
$ python3.XX -m venv cs-eval-env
$ source cs-eval-env/bin/activate
```

여기서 `XX`는 사용하고자 하는 Python 버전입니다(예: `python3.13`). 새로 만든 환경이 비어 있는지는 `uv`를 사용하여 확인할 수 있습니다(사용 중이어야 함):

```bash
$ uv list
```

````

[python-venv]: https://docs.python.org/3/library/venv.html

예를 들어 `paper_evaluation` 디렉토리에서:

```bash
$ uv sync
```

### 평가 의존성

"clean" 환경에 들어간 후 평가 세트를 위한 요구 사항을 설치하세요:

```bash
$ uv sync
```

```{note}
이전 단계에 설치된 종속성(예: `torch`, `numpy` 등)을 일부 다운그레이드할 수 있습니다.
```

### 평가 모델

재현성을 위해 필요한 사전 학습된 모델 가중치는 `get_model` 함수를 통해 이용할 수 있습니다:

```python
>>> from cellSAM import get_model
>>> get_model();
```

가장 최신의 사전 학습된 모델 가중치를 자동으로 다운로드하고 압축을 풉니다.

```{admonition} 모델 버전
:class: note dropdown

`get_model`에 `version=` 키워드 인자를 사용하여 특정 모델 버전을 지정할 수 있습니다.

 - 버전 `1.2`는 논문에 게재된 결과를 생성하는 데 사용된 버전
 - 버전 `1.2`는 재현성 워크플로우와 호환되는 *최소* 모델 버전
```
```

### 평가 데이터셋

평가 데이터셋이 있는지 확인하세요. 다음을 통해 다운로드할 수 있습니다:

```python
>>> from cellSAM import download_training_data
>>> download_training_data()
```

압축된 데이터 아카이브 다운로드를 시작합니다. 압축된 데이터는 `$HOME/.cellsam_hf/datasets/cellsam-data_v{X.Y}.tar.gz`에 다운로드됩니다. 여기서 `X.Y`는 요청한 데이터셋 버전입니다.

압축을 풉�니다/uncompresses the dataset to a desired location.

```{admonition} 데이터셋 크기
:class: caution dropdown

압축된 데이터 아카이브는 14GB이며, 풀리면 84GB가 됩니다. 따라서 데이터를 다른 위치에 푸는 것을 권장합니다. 마찬가지로, 압축 해제는 계산적으로 집중적이며 병렬 압축 알고리즘의 이점을 볼 수 있습니다.

데이터를 `/data`에 8개의 스레드로 압축 해제하는 예시 Incantation:

```bash
$ tar --use-compress-program="unpigz -p 8" -xf $HOME/.cellsam_hf/datasets/cellsam-data_v1.2.tar.gz -C /data
```

풀린 데이터는 `/data/cellsam_v1.2`에서 사용할 수 있습니다.
```
```
```

## 평가 실행

모든 위의 단계를 완료한 후 `all_runs.sh` 쉘 스크립트를 통해 평가를 실행할 수 있습니다. 모델/데이터셋의 위치는 파일의 최상단 변수가 반영되도록 하세요. 위에서 기본값을 사용한 경우(그리고 기본 위치에 데이터셋을 푼 경우) 이미 해당 사항이 적용됩니다.

```bash
$ ./all_runs.sh
```

각 실행의 결과는 `summary.csv`에 로컬로 저장되며, 해당 데이터셋, 사용된 모델, 그리고 `f1_mean`을 기록합니다.

### 개별 평가

전체 평가 세트를 실행할 필요는 없습니다. 특정 데이터셋에 대한 평가를 제한할 수 있습니다.

`all_runs.sh`를 통해 일반적인 아이디어를 얻으려면 `eval_main.py`를 사용하는 방법을 확인하세요.
```