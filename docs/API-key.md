모델 및 데이터셋
================

사전 학습된 모델과 훈련 데이터셋은 비상업적 학술 용도로만 사용 가능한 [수정된 애플리스 라이선스][license]에 따라 제공됩니다.

[license]: https://github.com/vanvalenlab/deepcell-auth/blob/main/ASSET_LICENSE

API 키 사용
------------

Hugging Face에서 발급받은 토큰은 환경 변수로 추가해야 합니다:

```bash
export HF_TOKEN=<token-from-huggingface>
```

이 줄은 로그인 시 Hugging Face 모델에 액세스하는 데 자동으로 권한을 부여하도록 쉘 구성 파일(예: ``.bashrc``, ``.zshrc``, ``.bash_profile`` 등)에 추가할 수 있습니다.

다운로드할 모델 가중치
---------------------

사전 학습된 모델 가중치를 실행에 사용하려면 필수입니다. 첫 번째 실행 시 `{func}`~cellSAM.cellsam_pipeline.cellsam_pipeline`을 통해 자동으로 ``$HOME/.cellsam_hf/models``에 다운로드됩니다.

인수를 주지 않고 `get_model`을 호출하여 가장 최신의 사전 학습된 모델 가중치를 다운로드할 수도 있습니다.

```python
>>> from cellSAM import get_model
>>> get_model()
```

압축된 데이터셋
----------------

```{warning}
훈련 데이터셋은 약 14GB이니, 다운로드 전에 충분한 공간과 네트워크 대역폭이 있는지 확인하세요.
```

 similarly, 로컬 사용을 위해 훈련 데이터는 다음과 같이 다운로드할 수 있습니다:

```python
>>> from cellSAM import download_training_data

>>> download_training_data()
```

압축된 데이터셋은 ``$HOME/.deepcell/data/cellsam/``에 저장됩니다.