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


## Model

학습에 사용한 모델은 CNN을 활용한 대표적인 이미지 분류 모델인 ResNet에 기반한 ResNet18을 전이 학습하는 방식으로 만들어졌다.


