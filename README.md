# Text Classification Task for Dacon Climate Technology Classification Competition
Dacon competition 자연어기반 기후기술분류 AI경진대회
- 대회개요 : https://dacon.io/competitions/official/235744/overview/description
- We used a number of KoBERT models for embedding multiple features(fields) and Voting for predicting result 
- KoBERT Baseline code : https://github.com/SKTBrain/KoBERT
- The dataset was given from dacon :)

# log
'21. 6. 26. </br>
자연어기반 기후기술분류 AI경진대회 참가를 위한 레퍼지토리 생성

'21. 7. 24. </br>
KoBERT model(v1.0) 업로드
- Test core : 0.73325(43rd) 
- 환경 : Colab Pro
- 개선방향
  1) text preprocessing -> make clean_text(sent) function
  2) epoch 증가(10까지)
  3) Validation set 만들어서 학습진행(학습 부분에 주석처리된 부분 되살리기)
  4) Imbalanced Data 해결해서 테스트해보기(균등하게 sampling 해서) 

'21. 7. 25. </br>
KoBERT model(v2.0) 업로드
- Test score : 0.74585(35th)
- 환경 : Colab Pro
- 반영사항
  1) 전처리 함수 추가(text_preprocessing)
  2) epoch 증가(10까지)
  3) Validation set 만들어서 학습진행
- 개선방향
  1) 다른 Feautre 들 추가해서 학습해보기(키워드 등은 효과가 좋을 것이라 예상) -> model을 2개로 나눠서 결과를 합치는 걸 생각할지, 기존 모델에 계속 학습시킬건지 고민해봐야함
  2) Hierarchical Classification 내용 확인해보고 모델 수정 검토
  3) 모델 불러와서 epoch 수 좀 더 늘려도 될 듯 함(validation score 봤을 때) 


'21. 7. 26. </br>
KoBERT model(v3.1) 업로드 실패
- Test score : 
- 환경 : Colab Pro
- 반영사항
  1) train, test dataset의 모든 column(6개)을 학습에 활용
- 개선방향
  1) CUDA memory 부족으로 학습이 안 됨!
  2) Data augmentation in NLP 읽어보고 반영여부 결정 (https://neptune.ai/blog/data-augmentation-nlp)
  
'21. 7. 27. </br>
Text classification 생각정리
* 우리가 학습시켜야 하는 데이터 처럼 Training Dataset 이 Multi features를 가질 때,
  1) 각각의 feature를 학습하는 모델 N개를 만들어 따로 학습시킨 뒤 predict 단계에서 N개의 모델 결과 값을 합쳐 최종 결과 값을 예측
  2) 모든 feature 를 concatenate 시킨 뒤 각각의 feature를 구분하는 특수 토큰을 삽입하고 한개의 모델에 학습시켜 결과 값을 예측
  둘 중에 뭐가 더 나은 방법일까. 1)의 경우 각 모델 학습시 MaxLen이 작아지기 때문에 memory 이슈로 부터 보다 자유롭고 아낀 메모리를 Batch size를 키우는데 사용할 수 있으므로 상대적으로 학습 시키는데 드는 시간이 적어짐. 반면에 특정 feature에 결측치가 너무 많은 경우 학습이 어려울 것이고, 결과 값을 합치는 함수도 뭐로 정해야 하는지 알아보긴 해야함... 2)의 경우 아이디어는 괜찮은데 Memory 이슈가 너무 크리티컬해서 현재상황에선 적용이 제한됨ㅜ 그렇지만 정말 큰 메모리를 할당받았을 때 둘중 어느 방식이 성능이 좋을지는 궁금함. 아래는 StackOverflow 에서 누군가 추천해준 논문
  
  * (관련자료)
  
  * 페이퍼 : https://arxiv.org/pdf/1903.04254.pdf
  
  * 추천 출처 :https://stackoverflow.com/questions/61252972/nlp-for-multi-feature-data-set-using-tensorflow
  
* 지금 주어진 task에서 데이터가 Hierarchical label 을 갖기 때문에 관련 자료를 읽어보고 우리 상황에 적용이 가능하다면 구현해볼 가치가 있다고 느낌.
  
  * (관련자료)
  
  * 페이퍼 : https://www.sciencedirect.com/science/article/pii/S0022000013000718
  
  * 아티클 : https://www.kdnuggets.com/2018/03/hierarchical-classification.html

'21. 8. 15. </br>
Issue : 성능이 안 나온다!
* 지난 한 주 동안 여러가지 모델들을 만들어 테스트 해봤는데 생각보다 성능이 잘 나오지 않음
  1) 6개의 features를 학습하기 위해 각 컬럼값을 하나의 data + label로 reshape 해준 뒤 하나의 BERT 모델에 학습하고, 각 feature들의 BERT embedding(768)을 결합해 FC layer 2개층을 통한 뒤 46개의 class를 예측해보려는 시도 -> 9 epoch 학습한 model로 테스트 했을 때 0.7279 의 스코어를 기록. 
  2)  6개의 각 fields를 별도의 공간에 embedding 하기 위해 6개의 KoBERT 모델에 별도로 학습시킨 뒤, 각 모델이 예측한 Class(46) 값을 모아 Voting 하는 시도 -> 15epoch 학습한 model로 0.6817 의 스코어를 기록
  3)  위 두가지 방법 모두 데이터 셋을 전처리 할 때 0 Class data의 10%만을 sampling해서 사용했다. 그 후 Validation set으로 20%를 떼어주고 난 뒤 데이터 갯수가 30000여개 정도에 불과했다. 
* 나름대로 열심히 생각했고, 학습하는데 시간도 많이 투자한 방법이었는데 두 방법 다 기존의 스코어를 넘어서지 못했다. 기존의 v2.0 model의 Validation set 정확도를 봤을 때 추가 학습 여지가 남아있었고 epoch수를 더 높이면 점수향상은 기대할 수 있다고 생각했지만 기본적으로 BERT 모델 자체를 제대로 이해하지 못하고 설계한 모형인 것 같아 다시 설계했는데 결과가 좋지 못했다.
* 우선, 0  class 10% Sampling의 효과가 없는듯했다. 왜냐하면, 기존 v2.0 설계가 Sentence 단위 Embedding에 특화된 BERT 모델의 특성상 연구과제명 한개의 feature 정도만 제대로 학습을 하고 분류에 이용했을 텐데 이 모델을 10 epoch 학습한 것보다 F1 score가 낮았기 때문이다. 혹시나 Voting이 문제인가 싶어 연구 과제명만 따로 분리하여 테스트를 진행했는데 결과는 0.64 정도로 Voting을 적용했을 떄보다 0.04 가량 낮은 F1 score가 나왔다. 결국, Voting은 효과가 있었고, Sampling은 되려 역효과가 나고 있다는 생각이 들었다. 그래서 다시 샘플링을 하지 않고 전처리만을 거친 데이터(Validation 분리 이후 140,000개 가량)를 가지고 각각 6개의 모델을 v2.0보다 더 많은 epoch(15)을 학습시킨 뒤 Voting 하여 결과를 제출해보자는 결론을 냈다. 아마 제출 날까지 학습만 시키기에도 시간이 빠듯하여(데이터가 140,000개나 되는 덕에 한개의 모델을 학습하는 데만 12시간 가량이 소요됨) 이 방법을 테스트 해보고 결과가 좋지 않더라도 더 이상의 개선은 힘들듯 함:)
