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
