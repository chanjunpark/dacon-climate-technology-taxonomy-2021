# dacon_climate_technology_classification
Dacon competition 자연어기반 기후기술분류 AI경진대회('21.6 ~ '21.8)

# log
'21. 6. 26. </br>
자연어기반 기후기술분류 AI경진대회 참가를 위한 레퍼지토리 생성

'21. 7. 24. </br>
KoBERT model(v1.0) 업로드 
- 환경 : Colab Pro
- 개선방향
  1) text preprocessing -> make clean_text(sent) function
  2) epoch 증가(10까지)
  3) Validation set 만들어서 학습진행(학습 부분에 주석처리된 부분 되살리기)
  4) Imbalanced Data 해결해서 테스트해보기(균등하게 sampling 해서) 
