# CNN을 통한 음식 내 알레르기 유발 재료 예측

### AI Project Team 9

본 프로젝트는 CNN을 활용하여 음식에 대한 이미지 데이터를 입력으로 받아 총 11개의 대표적인 알레르기 유발 식재료에 대한 포함 여부를 예측한다.

이를 통해 사용자로 하여금 사전에 위험 여부를 알려줌으로써, 음식을 주문하거나 구매할 때 간편한 방식으로 안전한 선택을 할 수 있게 해주는 것을 목표로 한다.

### 팀원

유욱철 (2018311182) - 팀장

김선규 (2017310041)

서현원 (2018314675)

김찬호 (2018312948)

박수환 (2018312229)

남궁수 (2019313519)

안우솔 (2018312723)


## Dataset

DATUMO Open Datasets에서 무료로 제공하는 음식 이미지 데이터셋인 [Food Image Dataset](https://open.datumo.com/en/?page_id=5976)을 사용하였다.
Food Image Dataset은 100가지 종류의 음식에 대하여 1024 x 1024 해상도의 이미지를 1000장 이상 제공하며, 각 이미지에 대한 정보를 담은 json 파일을 제공한다.

json 파일의 구성 예시는 다음과 같다.

    {
        "instance_num": 11,
        "country": "italy",
        "food_class": "pasta",
        "ingredients": [
            {
                "subtype": "meat",
                "ingredient": "processed meat"
            },
            {
                "subtype": "vegetable",
                "ingredient": "parsley"
            },
            {
                "subtype": "soup_sauce_spice",
                "ingredient": "tomato_sauce"
            },
            {
                "subtype": "grain_nuts",
                "ingredient": "pasta_noodle"
            }
        ]
    }

압축 해제 과정에서 오류가 발생하지 않은 91개 클래스를 사용하였고 모델의 입력으로 사용할 이미지의 크기 역시 용량 문제로 인해 128 x 128 해상도로 축소 하였다.
이후에는 json 데이터를 기반으로 학습 및 평가용 데이터를 csv 파일로 생성하였다.

본 프로젝트의 경우 다양한 알레르기 유발 재료 중 11가지 재료 (egg, chicken, shrimp, cheese, pork, crab, cream, tofu, lobster, peanut, bread)에 대하여 예측하는 모델 생성을 목표로 하기 때문에, 해당 재료들이 존재하는지 여부를 추출하여 포함된 경우 1로 없는 경우 0으로 표현하였다.

## Model

학습에 사용한 모델은 CNN을 활용한 대표적인 이미지 분류 모델인 ResNet에 기반한 ResNet18을 전이 학습하는 방식으로 만들어졌다.

ResNet은 Residual Network의 줄임말로 신경망이 깊어질수록 발생하는 기울기 소실 문제와 레이어가 많을수록 학습 오류가 증가하는 문제를 해결하기 위해 하나 이상의 레이어를 건너 뛰는 Shortcut Connection을 만들어 Residual Block이라는 새로운 구조를 형성하는 신경망 구조이다. ResNet18은 총 18개의 가중치가 있는 레이어를 사용한 경우를 말한다. (논문 Deep Residual Learning for Image Recognition 참조)

본 프로젝트는 2가지 구조에 대한 모델을 제시한다. 기본적으로 위에서 소개한 ResNet18을 사전 학습한 모델을 가져와 전이 학습을 한다는 공통점이 존재하지만, 분류 방식에서의 차이를 둔 구조이다.

첫 번째 구조는 multi label classification을 하는 구조로, 이미지를 입력으로 받아 11개의 재료에 대하여 각 재료가 존재할 확률을 나타내는 길이 11짜리 벡터를 출력한다.

![image](https://github.com/mygummy/AI_Project_Team9/assets/50257084/7d7048b2-b5b6-4620-aaf3-c666edd7d4a7)

두 번째 구조는 각 재료에 대하여 ResNet18을 두고, 각각에 대한 존재 확률을 하나의 스칼라로 반환한다. 즉, 11개의 독립된 모델의 결과를 기반으로 주어진 음식에 대한 각 재료의 포함 여부를 결정하게 된다.

![image](https://github.com/mygummy/AI_Project_Team9/assets/50257084/82cb0208-5d00-4a21-a167-b411d11ba443)

두 모델 중에 각각의 재료를 독립적으로 처리하는 모델보다 Multi Label Classification을 하는 모델이 더 높은 성능을 보일 것이라는 가정하고, Baseline Model은 각 Label에 대한 독립적인 모델을 학습시킨 방식을 선정하였다.
