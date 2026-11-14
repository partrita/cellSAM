**Anchor DETR**: Query Design for Transformer-Based Detector
============================================================

## 소개
이 리포지토리는 [Anchor DETR](https://arxiv.org/abs/2109.07107)의 공식 구현입니다. 앵커 포인트를 DETR의 객체 쿼리로 인코딩합니다. 각 앵커 포인트에 여러 패턴을 연결하여 "하나의 영역, 여러 객체"의 어려움을 해결합니다. 또한 고해상도 특징에 대한 메모리 비용을 절감하기 위해 attention 변형인 RCDA도 제안합니다.

![DETR](.github/pipeline.png)


## 주요 결과

|                    | 특징       |  에폭 |  AP     |  GFLOPs  | 추론 속도 (FPS) |
|:------------------:|:------------:|:------:|:-------:|:--------:|:----------------:|
| DETR               |  DC5         |  500   |  43.3   |  187     | 10 (12)          |
| SMCA               |  multi-level |  50    |  43.7   |  152     | 10               |
| Deformable DETR    |  multi-level |  50    |  43.8   |  173     | 15               |
| Conditional DETR   |  DC5         |  50    |  43.8   |  195     | 10               |
| Anchor DETR        |  DC5         |  50    |  44.3   |  172     | 16 (19)          |

*참고:*
1. 결과는 ResNet-50 백본을 기반으로 합니다.
2. 추론 속도는 NVIDIA Tesla V100 GPU에서 측정되었습니다.
3. FPS의 괄호 안 값은 torchscript 최적화 속도를 나타냅니다.


## 모델
| 이름             | 백본  |  AP     |  URL  |
|:----------------:|:-------:|:-------:|:-----:|
| AnchorDETR-C5    |  R50    |  42.1   | [모델](https://drive.google.com/file/d/1ktLJyw4PGdaXkOn61W537Z67WHcttXDs/view?usp=sharing) / [로그](https://drive.google.com/file/d/1CoEUzs6pxYw-z1ew04qC1jFJwVjdDlPv/view?usp=sharing) |
| AnchorDETR-DC5   |  R50    |  44.3   | [모델](https://drive.google.com/file/d/1lJZWdIlHj6KKmAdU28Y01tTyO0hc6Jxs/view?usp=sharing) / [로그](https://drive.google.com/file/d/1ywmE02P7ORj_1HQOR2lYW11kfuqX00v-/view?usp=sharing) |
| AnchorDETR-C5    |  R101   |  43.5   | [모델](https://drive.google.com/file/d/1eBLYzlKWwSF_RRcfjgRXqIplRKetsvtg/view?usp=sharing) / [로그](https://drive.google.com/file/d/1XIDSpYCioYlK5NwdJnbUHQls-PUr_xwi/view?usp=sharing) |
| AnchorDETR-DC5   |  R101   |  45.1   | [모델](https://drive.google.com/file/d/1irmZPSALME4Nht3_qhM9WLExDyO9Sj-J/view?usp=sharing) / [로그](https://drive.google.com/file/d/1KIIYid8mmoAWX7w6T6VPhORc86STqoXR/view?usp=sharing) |

*참고:* 모델과 로그는 [Baidu Netdisk](https://pan.baidu.s/1iB8qtVPb9dWHYgA5z1I4xg)와 코드 `hh13`에서도 이용할 수 있습니다.

## 사용법

### 설치
먼저 로컬에 리포지토리를 클론합니다:
```
git clone https://github.com/megvii-research/AnchorDETR.git
```
그런 다음 의존성을 설치합니다:
```
uv install -r requirements.txt
```

### 훈련
8개의 GPU가 있는 싱글 노드에서 AnchorDETR을 훈련하려면:
```
python -m torch.distributed.launch --nproc_per_node=8 --use_env main.py  --coco_path /path/to/coco 
```

### 평가
8개의 GPU가 있는 싱글 노드에서 AnchorDETR을 평가하려면:
```
python -m torch.distributed.launch --nproc_per_node=8 --use_env main.py --eval --coco_path /path/to/coco --resume /path/to/checkpoint.pth 
```

단일 GPU로 AnchorDETR을 평가하려면:
```
python main.py --eval --coco_path /path/to/coco --resume /path/to/checkpoint.pth
```


## 인용
이 프로젝트가 연구에 유용하다고 생각되면 인용을 고려해 주세요.
```
@inproceedings{wang2022anchor,
  title={Anchor detr: Query design for transformer-based detector},
  author={Wang, Yingming and Zhang, Xiangyu and Yang, Tong and Sun, Jian},
  booktitle={Proceedings of the AAAI Conference on Artificial Intelligence},
  volume={36},
  number={3},
  pages={2567--2575},
  year={2022}
}
```

## 문의
질문이 있으면 자유롭게 이슈를 열거나 wangyingming@megvii.com으로 연락주세요.