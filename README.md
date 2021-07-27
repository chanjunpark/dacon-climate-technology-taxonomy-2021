# dacon_climate_technology_classification
Dacon competition 자연어기반 기후기술분류 AI경진대회
- 주최 : 녹색기술센터(GCT)
- 대회기간 : '21.6 ~ '21.8.16.
- 평가산식 : Macro-F1

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
- 우리가 학습시켜야 하는 데이터 처럼 Training Dataset 이 Multi features를 가질 때,
  1) 각각의 feature를 학습하는 모델 N개를 만들어 따로 학습시킨 뒤 predict 단계에서 N개의 모델 결과 값을 합쳐 최종 결과 값을 예측
  2) 모든 feature 를 concatenate 시킨 뒤 각각의 feature를 구분하는 특수 토큰을 삽입하고 한개의 모델에 학습시켜 결과 값을 예측
  둘 중에 뭐가 더 나은 방법일까. 1)의 경우 각 모델 학습시 MaxLen이 작아지기 때문에 memory 이슈로 부터 보다 자유롭고 아낀 메모리를 Batch size를 키우는데 사용할 수 있으므로 상대적으로 학습 시키는데 드는 시간이 적어짐. 반면에 특정 feature에 결측치가 너무 많은 경우 학습이 어려울 것이고, 결과 값을 합치는 함수도 뭐로 정해야 하는지 알아보긴 해야함... 2)의 경우 아이디어는 괜찮은데 Memory 이슈가 너무 크리티컬해서 현재상황에선 적용이 제한됨ㅜ 그렇지만 정말 큰 메모리를 할당받았을 때 둘중 어느 방식이 성능이 좋을지는 궁금함. 아래는 StackOverflow 에서 누군가 추천해준 논문
  (관련자료)
  페이퍼 : https://arxiv.org/pdf/1903.04254.pdf
  추천 출처 :https://stackoverflow.com/questions/61252972/nlp-for-multi-feature-data-set-using-tensorflow
  
- 지금 주어진 task에서 데이터가 Hierarchical label 을 갖기 때문에 관련 자료를 읽어보고 우리 상황에 적용이 가능하다면 구현해볼 가치가 있다고 느낌.
  (관련자료)
  페이퍼 : https://www.sciencedirect.com/science/article/pii/S0022000013000718
  아티클 : https://www.kdnuggets.com/2018/03/hierarchical-classification.html
