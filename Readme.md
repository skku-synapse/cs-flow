# CS-Flow

CS-Flow 논문 : WACV 2022 paper "[Fully Convolutional Cross-Scale-Flows for Image-based Defect Detection](https://arxiv.org/pdf/2110.02855.pdf)" by Marco Rudolph, Tom Wehrbein, Bodo Rosenhahn and Bastian Wandt.

## 1. CS-Flow Official Document

CS-Flow의 공식 document 입니다.  
환경 세팅 방법, 데이터 세팅 방법, 코드 실행 방법 등이 설명되어 있습니다.

### 1-1) Getting Started

You will need [Python 3.6](https://www.python.org/downloads) and the packages specified in _requirements.txt_.
We recommend setting up a [virtual environment with pip](https://packaging.python.org/guides/installing-using-pip-and-virtual-environments/)
and installing the packages there.

Install packages with:

```
$ pip install -r requirements.txt
```

### 1-2) Configure and Run

All configurations concerning data, model, training, visualization etc. can be made in _config.py_. The default configuration will run a training with paper-given parameters on the provided dummy dataset. This dataset contains images of 4 squares as normal examples and 4 circles as anomaly.

To extract features, run extract*features.py (this was already done here for the dummy dataset, features were extracted to data/features).
To start the training, just run \_main.py*!
Please report us if you have issues when using the code.

### 1-3) Data

The given dummy dataset shows how the implementation expects the construction of a dataset. Coincidentally, the [MVTec AD dataset](https://www.mvtec.com/company/research/datasets/mvtec-ad) is constructed in this way.

Set the variables _dataset_path_ and _class_name_ in _config.py_ to run experiments on a dataset of your choice. The expected structure of the data is as follows:

```
train data:

        dataset_path/class_name/train/good/any_filename.png
        dataset_path/class_name/train/good/another_filename.tif
        dataset_path/class_name/train/good/xyz.png
        [...]

test data:

    'normal data' = non-anomalies

        dataset_path/class_name/test/good/name_the_file_as_you_like_as_long_as_there_is_an_image_extension.webp
        dataset_path/class_name/test/good/did_you_know_the_image_extension_webp?.png
        dataset_path/class_name/test/good/did_you_know_that_filenames_may_contain_question_marks????.png
        dataset_path/class_name/test/good/dont_know_how_it_is_with_windows.png
        dataset_path/class_name/test/good/just_dont_use_windows_for_this.png
        [...]

    anomalies - assume there are anomaly classes 'crack' and 'curved'

        dataset_path/class_name/test/crack/dat_crack_damn.png
        dataset_path/class_name/test/crack/let_it_crack.png
        dataset_path/class_name/test/crack/writing_docs_is_fun.png
        [...]

        dataset_path/class_name/test/curved/wont_make_a_difference_if_you_put_all_anomalies_in_one_class.png
        dataset_path/class_name/test/curved/but_this_code_is_practicable_for_the_mvtec_dataset.png
        [...]
```

### 1-4) Credits

Some code of an old version of the [FrEIA framework](https://github.com/VLL-HD/FrEIA) was used for the implementation of Normalizing Flows. Follow [their tutorial](https://github.com/VLL-HD/FrEIA) if you need more documentation about it.

### 1-5) Citation

Please cite our paper in your publications if it helps your research. Even if it does not, you are welcome to cite us.

        @inproceedings { RudWeh2022,
        author = {Marco Rudolph and Tom Wehrbein and Bodo Rosenhahn and Bastian Wandt},
        title = {Fully Convolutional Cross-Scale-Flows for Image-based Defect Detection},
        booktitle = {Winter Conference on Applications of Computer Vision (WACV)},
        year = {2022},
        url = {arxiv},
        month = jan
        }

### 1-6) License

This project is licensed under the MIT License.

## 2. 변경 사항

### 2-1) Anomaly Score 산출 방식 변경

평균 -> 표준편차 변경  
참고 파일: evaluate.py, train.py

### 2-2) 모델 저장 및 불러오기 방식 변경

전체 모델 저장 -> 모델 파라미터만 저장  
참고 파일: model.py, train.py

### 2-3) configuration file 변경

| 변수명            | 설명                                                         | 기존 값    | 변경된 값  |
| ----------------- | ------------------------------------------------------------ | ---------- | ---------- |
| img_size          | 가장 큰 스케일 기준 이미지 사이즈                            | (768, 768) | (512, 512) |
| n_coupling_blocks | normalizing flow의 반복되는 coupling block 수                | 4          | 1          |
| fc_internal       | cross scale convolution channel 수 (hidden layer channel 수) | 1024       | 512        |

참고 파일: config.py

## 3. hyper-parameters

| 변수명      | 설명                           | 값     |
| ----------- | ------------------------------ | ------ |
| batch_size  | batch size                     | 16     |
| lr_init     | learning rate                  | 2e-4   |
| sub_epochs  | 1 sub_epoch 마다 evaluate 진행 | 10     |
| meta_epochs | sub epochs 반복 횟수           | 50~100 |

참고 파일: config.py
