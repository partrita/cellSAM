CellSAM 문서
==============

CellSAM는 세포 세그먼테이션을 위한 범용 모델입니다.

CellSAM는 박테리아, 조직, 효모, 세포 배양 등 다양한 세포 표적 및 영상 모달리티(밝은 빛, 형광, 위상, 멀티플렉스 등)에서 최신 성능을 달성합니다.

모델 및 일반적인 접근 방식에 대한 자세한 내용은 `사전 인쇄`_를 참조하세요.

.. _사전 인쇄: https://www.biorxiv.org/content/10.1101/2023.11.17.567630v3

설치
--------

.. code-block:: bash

   uv sync

.. currentmodule:: cellSAM.cellsam_pipeline

기본 사용법
------------

primary 인터페이스는 :func:`cellsam_pipeline`으로, CellSAM 모델의 추론 파이프라인을 구현합니다.

예시 갤러리에서 CellSAM가 다양한 이미지에 적용된 예를 확인하세요 :ref:`example_gallery`.

인용
------

.. code-block:: latex

   @article{israel2023foundation,
     title={A Foundation Model for Cell Segmentation},
     author={Israel, Uriah and Marks, Markus and Dilip, Rohit and Li, Qilin and Schwartz, Morgan and Pradhan, Elora and Pao, Edward and Li, Shenyi and Pearson-Goulart, Alexander and Perona, Pietro and others},
     journal={bioRxiv},
     publisher={Cold Spring Harbor Laboratory Preprints},
     doi = {10.1101/2023.11.17.567630},
   }

.. toctree::
   :maxdepth: 1
   :hidden:

   tutorial
   API-key
   reference/index
   auto_examples/index
   napari
   reproducibility