### An Ensemble Approach of Recurrent Neural Networks using Pre-Trained Embeddings for Playlist Completion
본 논문은 트랙, 아티스트, 앨범, 노래제목을 input으로 하여, embedding representation을 사전학습하고, 이를 활용한 다양한 RNN으로 앙상블하는 모델을 제안한다. 또한 semantic과 stylistic feature로부터 추출한 가사를 새로운 트랙을 만드는데 사용한다. 트랙이 없는 플레이리스트의 트랙을 추가하기 위하여 제목으로 트랙을 추천하는 Title2Rec이라는 대체 모델을 생성했다. (RecSys Challenge 2018)


### TrailMix: An Ensemble Recommender System for Playlist Curation and Continuation

TrailMix는 세가지 모델의 결합이다. playlist 제목을 위한 cluster 기반의 접근법인 CC-title, track간의 상호작용을 활용한 Neural Collaborative Filtering인 DNCF, track간의 관계를 찾기위한 계층적 접근방식인 C-Tree를 결합한 모델이다.
