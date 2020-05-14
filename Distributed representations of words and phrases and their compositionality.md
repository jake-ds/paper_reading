# Distributed representations of words and phrases and their compositionality

Class: NLP
Created: May 14, 2020 9:28 PM
Reviewed: No

최근(2013년)에 발표된 **Skip-gram 모델의 성능향상(feature vector의 질과 학습속도)을 위한 몇가지 방법을 제안**한다.

- 다빈도 단어에 subsampling을 적용하여 학습속도 향상 및 일반화된 분산표현 학습
- hierarchical softmax의 대안으로 negative sampling 제시
- 관용어 처리를 위한 구문(phrase)학습 방법 제시

단어의 분산표현은 1986년에 최초로 도입되었고, 통계적언어모델에 성공적으로 적용하였다. 이후 음성인식, 기계번역 등 NLP task에 널리 사용되었다.

# Skip-gram

**중심 단어를 통해 주변단어를 예측하는 모델**. CBOW(Continuos bag of word: 주변단어로 중심단어를 예측)와 달리 중심단어가 주변단어와 완전히 독립이라고 가정한다.

<div>
<img width="400", src = "https://mail.google.com/mail/u/0?ui=2&ik=91531500aa&attid=0.2&permmsgid=msg-f:1666654586044621034&th=17212584047720ea&view=fimg&sz=s0-l75-ft&attbid=ANGjdJ9fzTUPPJwLsaIigFvLK5aKsydtjqjp1Z9u4CgtVKcjl_ioLGPEGL0GZfk379ZWlydrPSisfeCLa0FG3UCRdlcoRlvlW1J8UHypUlSqa3tfDLAd6YiApJEXqpM&disp=emb"
</div>
(https://mail.google.com/mail/u/0?ui=2&ik=91531500aa&attid=0.2&permmsgid=msg-f:1666654586044621034&th=17212584047720ea&view=fimg&sz=s0-l75ft&attbid=ANGjdJ9fzTUPPJwLsaIigFvLK5aKsydtjqjp1Z9u4CgtVKcjl_ioLGPEGL0GZfk379ZWlydrPSisfeCLa0FG3UCRdlcoRlvlW1J8UHypUlSqa3tfDLAd6YiApJEXqpM&disp=emb)

[https://mail.google.com/mail/u/0?ui=2&ik=91531500aa&attid=0.3&permmsgid=msg-f:1666654586044621034&th=17212584047720ea&view=fimg&sz=s0-l75-ft&attbid=ANGjdJ__HyqO-PD5VKEFbG4ujIQ4NS68ZL1Xr-MhtxgJvnVXsb9FLdgcKaqaQK6YLipym2p0C7OurKRn4VAuQXBEIBPJ3YYtSAuD6C2Hqm_0oQ5FBZqM9ZiajuDAcOM&disp=emb](https://mail.google.com/mail/u/0?ui=2&ik=91531500aa&attid=0.3&permmsgid=msg-f:1666654586044621034&th=17212584047720ea&view=fimg&sz=s0-l75-ft&attbid=ANGjdJ__HyqO-PD5VKEFbG4ujIQ4NS68ZL1Xr-MhtxgJvnVXsb9FLdgcKaqaQK6YLipym2p0C7OurKRn4VAuQXBEIBPJ3YYtSAuD6C2Hqm_0oQ5FBZqM9ZiajuDAcOM&disp=emb)

skip-gram의 목표는 **중심단어를 기준으로 해서 특정 간격 내에 있는 단어들이 등장할 확률의 합을 최대화** 하는 것이다.

따라서, training samples의 개별 확률의 합을 최대화하는 방향으로 학습을 진행한다.

[https://mail.google.com/mail/u/0?ui=2&ik=91531500aa&attid=0.4&permmsgid=msg-f:1666654586044621034&th=17212584047720ea&view=fimg&sz=s0-l75-ft&attbid=ANGjdJ_92Lg7tCyZ3bme60A2idlF_fRmfKrI3Xp8SypsEBCrExhr3MkPC0kagGj4ks5pbz5NCxt2ofvQN9Wrr5dBBseloNbAfP3K_S4o0OEIJX77eMEySzbZ3EHEFnQ&disp=emb](https://mail.google.com/mail/u/0?ui=2&ik=91531500aa&attid=0.4&permmsgid=msg-f:1666654586044621034&th=17212584047720ea&view=fimg&sz=s0-l75-ft&attbid=ANGjdJ_92Lg7tCyZ3bme60A2idlF_fRmfKrI3Xp8SypsEBCrExhr3MkPC0kagGj4ks5pbz5NCxt2ofvQN9Wrr5dBBseloNbAfP3K_S4o0OEIJX77eMEySzbZ3EHEFnQ&disp=emb)

[https://mail.google.com/mail/u/0?ui=2&ik=91531500aa&attid=0.5&permmsgid=msg-f:1666654586044621034&th=17212584047720ea&view=fimg&sz=s0-l75-ft&attbid=ANGjdJ8a8xz2q_HAjOI_asRsqXxGXFAXnDGd0_uRMvkhQt_TMooJBXdOddipSKVGbxsIl7yfYYvK6SoZqfJRRgdA0_fTIlpp558Pyw3HhuDrLjERdv2ANAghDUzwVIk&disp=emb](https://mail.google.com/mail/u/0?ui=2&ik=91531500aa&attid=0.5&permmsgid=msg-f:1666654586044621034&th=17212584047720ea&view=fimg&sz=s0-l75-ft&attbid=ANGjdJ8a8xz2q_HAjOI_asRsqXxGXFAXnDGd0_uRMvkhQt_TMooJBXdOddipSKVGbxsIl7yfYYvK6SoZqfJRRgdA0_fTIlpp558Pyw3HhuDrLjERdv2ANAghDUzwVIk&disp=emb)

[https://mail.google.com/mail/u/0?ui=2&ik=91531500aa&attid=0.6&permmsgid=msg-f:1666654586044621034&th=17212584047720ea&view=fimg&sz=s0-l75-ft&attbid=ANGjdJ_QwEAtr_EG_awXl9I9SV2GxWtWkU1el0xP1BQ9wtjpN2gla6uRtuf358AcXvFEH5GIMLizN7qync_sAHDGREoQ72eB-Y4r3P6W8FqcctBlbqNxahZuTvjEDKE&disp=emb](https://mail.google.com/mail/u/0?ui=2&ik=91531500aa&attid=0.6&permmsgid=msg-f:1666654586044621034&th=17212584047720ea&view=fimg&sz=s0-l75-ft&attbid=ANGjdJ_QwEAtr_EG_awXl9I9SV2GxWtWkU1el0xP1BQ9wtjpN2gla6uRtuf358AcXvFEH5GIMLizN7qync_sAHDGREoQ72eB-Y4r3P6W8FqcctBlbqNxahZuTvjEDKE&disp=emb)

위의 예에서 <fox>가 나왔을 때, <quick>이 나올 확률 더하기 <fox>가 나왔을 때, <brown>이 나올 확률 더하기 .....(계속)

이런 방식으로 중심단어별 구한 확률의 합을 모두 더한 값을 최대화 시킨다.

[https://mail.google.com/mail/u/0?ui=2&ik=91531500aa&attid=0.7&permmsgid=msg-f:1666654586044621034&th=17212584047720ea&view=fimg&sz=s0-l75-ft&attbid=ANGjdJ8RJ9s45grJnoURrc8AEYjcf3n-lUxxASlj2UT9X68KIc_DL3rL5e_j9Tt936ofzc_6pT9GHVatj1LEe4yrG-9pKl90UtYnirCt5S2mg8UZ2KxhgW0kejfLaFY&disp=emb](https://mail.google.com/mail/u/0?ui=2&ik=91531500aa&attid=0.7&permmsgid=msg-f:1666654586044621034&th=17212584047720ea&view=fimg&sz=s0-l75-ft&attbid=ANGjdJ8RJ9s45grJnoURrc8AEYjcf3n-lUxxASlj2UT9X68KIc_DL3rL5e_j9Tt936ofzc_6pT9GHVatj1LEe4yrG-9pKl90UtYnirCt5S2mg8UZ2KxhgW0kejfLaFY&disp=emb)

**중심단어와 주변단어가 함께 등장할 확률을 구할 때, softmax function을 사용한다. 이 방식은 매우 많은 양의 연산을 필요로 하기에 실용적이지 않다.** skip-gram에서 training sample의 확률을 구할 때, 연산량이 적고 실용적인 방법을 제시한다.

## **1) Hierarchical softmax**

이 방식은 계산량이 많은 Softmax function 대신 보다 빠르게 계산 가능한 multinomial distribution function을 사용하는 방식이다. 각 단어들을 노드로 하는 binary tree를 구성한 후에 특정 단어의 확률을 구할 때, **경로를 따라가며 경로상의 각 단어의 확률을 곱하는 방식으로 최종단어의 확률을 구하는 방식이다**. 기존의 softmax는 확률을 구하기 위해, 모든 결과에 대한 합을 1로 만들어 주는 과정의 연산이 굉장히 복잡하였다. 하지만 이 방법은 노드가 깊어질 때, 두 개의 노드로 갈 확률이 각각 p, 1-p이므로 그 합이 1이 된다. 따라서 같은 깊이에서의 각 단어의 확률 값의 합이 항상 1이다.**결과(확률)의 합이 1이 되도록하는 normalize 연산이 없기 때문에 일반적인 softmax에 비해 효율적이다.**

실제로 기존의 방식에서는 W(number of vocabulary) 노드에 대해서 확률을 구해야 했다면, hierarchical softmax는 log2(W) 노드에 대해서만 확률을 구하기 때문에 연산량이 적다.

[https://mail.google.com/mail/u/0?ui=2&ik=91531500aa&attid=0.8&permmsgid=msg-f:1666654586044621034&th=17212584047720ea&view=fimg&sz=s0-l75-ft&attbid=ANGjdJ_nnZWWaJe1G2e3F_9K_YaBXSciURg4CTmlVPQeb0QcaqQd_jLF9JRS8p_QAZ6vIWjMV574aqVLbxQjmc-nNDguMACHj17S1BOaJwWW5d9eBiOYqbuwJxNM_qE&disp=emb](https://mail.google.com/mail/u/0?ui=2&ik=91531500aa&attid=0.8&permmsgid=msg-f:1666654586044621034&th=17212584047720ea&view=fimg&sz=s0-l75-ft&attbid=ANGjdJ_nnZWWaJe1G2e3F_9K_YaBXSciURg4CTmlVPQeb0QcaqQd_jLF9JRS8p_QAZ6vIWjMV574aqVLbxQjmc-nNDguMACHj17S1BOaJwWW5d9eBiOYqbuwJxNM_qE&disp=emb)

tree를 구하는 방식은 다양하다.

1) J.Goodman(2001)

학습 속도를 높이기 위하여, 2 or 3 level의 tree를 사용한다. (???)

2) F.Morin & Y.Bengio(2005)

Heuristic Felix 방식을 사용하여 그래프 생성 후 binary tree로 변경하기 위하여 th-idf 행렬을 사용하여 자식 노드를 클러스터링

3) A. Mnih&G.Hinton(2009)

boot-strapping 방식을 사용하서 tree 구축한다. 최초에는 랜덤tree를 사용하여 학습시키고, 이후 모든 단어의 context vector의 평균을 계산한다. 이후 context vector를 Gaussian mixture 모델을 사용하여 재귀적으로 파티셔팅한다.

## **2) Noise Contrastive Estimation(NCE) & Negative Sampling(NEG)**

(1) Noise Contrastive Estimation(NCE)

hierarchical softmax가 해결하고자 했던 문제와 마찬가지로, NCE 또한 같은 문제를 해결하고자한다. 일반적인 softmax가 최종 확률 값을 구하기 위하여 모든 단어의 확률을 합하는데 이 때 연산량이 크므로 이를 해결하고자한다.

이 문제를 해결하기 위하여 기댓값을 근사시키는 Monte Carlo을 활용한 importance sampling이 사용되었다. NCE는 이를 발전시킨 기법이다. NCE는 연산량을 줄이기 위하여 문제를 **binary classification proxy 문제로 변환**한다. noise distribution을 통해 생성된 데이터와 실제 데이터를 구별하는 classifier의 모수를 추정하는 문제로 변환한다.이 때 **parameter 수의 변화는 없지만 통계적으로 계산하기 수월해진다.**

(2) Negative Sampling(NEG)

학습시에 과도하게 많은 매개변수를 학습하므로 연산량이 급격히 많아지는 것을 방어하기 위한 방법이다. 일반적으로 전체 Corpus에는 중심단어와 관계가 없는 단어가 관계있는 단어에 비해 훨씬 많다. 그런데, 중심단어와 관계없는 단어는 학습을 하는 것이 큰 변화를 주지 않기 때문에, **중심단어와 관계없는 단어는 일부만 뽑아서 학습에 사용하는 방식**이다. NEG에서 Negative sample의 갯수가 전체 vocabulary수와 같다면, 이는 NCE 방법이 된다.

***결론(Note on Noist Contrastive Estimation and Negative Sampling / 1410.8251.pdf)

NCE는 임의의 지역적으로 normaliz된 언어모델의 파라미터를 학습하는데 효과적인 방법이다. 반면, NEG는 단어의 분산표현을 생성하는 대안적인 방법으로 보아야한다. 따라서 언어모델링을 목표로 한다면 NCE를 쓰고, 단어의 분산표현을 학습하는 것을 목표로 한다면 NCE와 NEG를 모두 고려하여야한다.

## **3) Subsampling of Requent Words**

위의 두 방식은 모델 자체의 계산복잡도를 줄이는 방법이었다면 그 방법은 단어를 확률적으로 제외하여 학습속도를 향상시켰다.

매우 큰 corpora에서는 “in”, “the”, and “a”와 같은 단어가 매우 빈번하게 발생하지만 실제로 이런 단어들이 담고있는 정보는 매우 적다. 반면 비교적 드물에 등장하는 단어에는 많은 정보가 담겨있다. 빈번하게 나타나는 단어와 드물게 나타나는 단어의 빈도수의 불균형을 해결하기 위한 방법으로 Subsampling을 제안한다. 이 방식은 **빈번하게 나타는 단어에 대해서는 샘플링할 때 해당 단어가 누락될 확률을 크게 주는 방식**이다.예를 들어 빈번하게 등장하는 "the"와 드물게 등장하는 "dog"가 있다면, 샘플링 할때 "the"가 탈락될 확률은 높고, "dog"가 탈락될 확률은 낮게 만들어주는 것이다. 각 단어의 제외될 확률은 아래와 같이 계산한다.

[https://mail.google.com/mail/u/0?ui=2&ik=91531500aa&attid=0.9&permmsgid=msg-f:1666654586044621034&th=17212584047720ea&view=fimg&sz=s0-l75-ft&attbid=ANGjdJ-SBo1y1tEPEgeR5ru2aIYJIzt1D1krXeIMCQ528ZWdYJyilW2Ko-HXhbmr0MVuNkJtcyHPxZ9MLdoQ3uVBLZNBDQbruGqUnsFUf4FC9I01pTsJbsUh1hJsNJc&disp=emb](https://mail.google.com/mail/u/0?ui=2&ik=91531500aa&attid=0.9&permmsgid=msg-f:1666654586044621034&th=17212584047720ea&view=fimg&sz=s0-l75-ft&attbid=ANGjdJ-SBo1y1tEPEgeR5ru2aIYJIzt1D1krXeIMCQ528ZWdYJyilW2Ko-HXhbmr0MVuNkJtcyHPxZ9MLdoQ3uVBLZNBDQbruGqUnsFUf4FC9I01pTsJbsUh1hJsNJc&disp=emb)

*아래 식에서 t는 파라미터로 heuristic하게 결정된다.

*f(wi)는 단어 wi의 빈도이다.

# Empirical Results

실증적인 실험을 위해 각 모델별로 유추 문제로 실험을 진행한다

**실험조건**

- 10억개 단어로 구성된 Google dataset
- 5번 미만 발생한 단어는 제외하고 69만 2천 단어 학습

[실험문제](https://www.notion.so/d10cb84b27b9493ba6ce598975f2b5a3)

**실험결과**

[https://mail.google.com/mail/u/0?ui=2&ik=91531500aa&attid=0.10&permmsgid=msg-f:1666654586044621034&th=17212584047720ea&view=fimg&sz=s0-l75-ft&attbid=ANGjdJ8SDEp1Yj6qk3V7Do-wsnF96VYMERFYIyRd5voCJrFf7DaUEWYyfzjcaap6xZFKz0ca1YksQdl6bWRcUpqEedU_KZNFaneC7AhrTwlxFyzQLeZn6SDMqiaKL1k&disp=emb](https://mail.google.com/mail/u/0?ui=2&ik=91531500aa&attid=0.10&permmsgid=msg-f:1666654586044621034&th=17212584047720ea&view=fimg&sz=s0-l75-ft&attbid=ANGjdJ8SDEp1Yj6qk3V7Do-wsnF96VYMERFYIyRd5voCJrFf7DaUEWYyfzjcaap6xZFKz0ca1YksQdl6bWRcUpqEedU_KZNFaneC7AhrTwlxFyzQLeZn6SDMqiaKL1k&disp=emb)

**NEG-k는 nagative sample k개 추출

**HS-Huffman : Huffman coding tree를 사용한 Hierarachical Softmax

유추실험에서 NEG모델이 HS-Huffman 모델보다 성능이 좋은 것으로 나타났다. 또한 NCE 보다 살짝 성능이 좋은 것으로 확인됐다.

또한 subsampling이 학습 속도를 향상 시키고, 정확도가 미세하게 상승하는 것을 확인할 수 있다.

skip-gram 모델이 선형적이므로 선형적인 유추 문제에 적합한 vector를 만든다. 그러나 standard sigmoidal recurrent neural network(고차원의 비선형 모델)로 학습된 모델 또한 학습데이터의 양이 증가할수록 선형적인 유추 문제에서 유의미한 향상을 보인다.

## **Learning Phrases**

구문을 학습하기 위하여, 구문을 하나의 token으로 변환하였다. "New York Times"와 같이 다른 단어와는 드물게 등장하지만 빈번하게 함께 등장하는 단어를 구문으로 보고 토큰화한다.("this is"와 같은 구문은 하나의 token으로 변환하지 않았다.) 이러한 방식으로 vocabulary의 크기를 증가시키지 않고, 많은 양의 phrase를 찾았다. 또한 실험을 위해서 bigram 모델을 선택하였다.

[https://mail.google.com/mail/u/0?ui=2&ik=91531500aa&attid=0.11&permmsgid=msg-f:1666654586044621034&th=17212584047720ea&view=fimg&sz=s0-l75-ft&attbid=ANGjdJ-RTg6hBY7uUFHN9Z1Ih3sPxLfpPTGK4AT6Ktfh2R9mRl9XS_o1edMB5u1RH0udwGS2jsMun3k_6PSUKnBi4hBWB5vJYvDpzONhjiXmK2tgHG2e-DLfuTDHhlk&disp=emb](https://mail.google.com/mail/u/0?ui=2&ik=91531500aa&attid=0.11&permmsgid=msg-f:1666654586044621034&th=17212584047720ea&view=fimg&sz=s0-l75-ft&attbid=ANGjdJ-RTg6hBY7uUFHN9Z1Ih3sPxLfpPTGK4AT6Ktfh2R9mRl9XS_o1edMB5u1RH0udwGS2jsMun3k_6PSUKnBi4hBWB5vJYvDpzONhjiXmK2tgHG2e-DLfuTDHhlk&disp=emb)

구문으로 판단하는데 사용하는 score 계산방식(bigram 기준)

: 각 단어의 빈도를 곱에 대한 두 단어가 함께 등장한 빈도의 비율이다.

**실험문제**

[https://mail.google.com/mail/u/0?ui=2&ik=91531500aa&attid=0.12&permmsgid=msg-f:1666654586044621034&th=17212584047720ea&view=fimg&sz=s0-l75-ft&attbid=ANGjdJ8wXhDBiTSPDA57NPuxXcwkVO9S6U9KpF6ss3DwMBgxdabpv8_O1Vbt3d1oZZfI35N17uJRIYWaqR_plMpPmoC4mDfINCDtqYUibpFCqMSIuf5jf6lGhDdGpuc&disp=emb](https://mail.google.com/mail/u/0?ui=2&ik=91531500aa&attid=0.12&permmsgid=msg-f:1666654586044621034&th=17212584047720ea&view=fimg&sz=s0-l75-ft&attbid=ANGjdJ8wXhDBiTSPDA57NPuxXcwkVO9S6U9KpF6ss3DwMBgxdabpv8_O1Vbt3d1oZZfI35N17uJRIYWaqR_plMpPmoC4mDfINCDtqYUibpFCqMSIuf5jf6lGhDdGpuc&disp=emb)

**실험결과**

[https://mail.google.com/mail/u/0?ui=2&ik=91531500aa&attid=0.13&permmsgid=msg-f:1666654586044621034&th=17212584047720ea&view=fimg&sz=s0-l75-ft&attbid=ANGjdJ9Fl6uwP0d1xS7K7Vs_MhhP7rWm3LNy2W9taJRedwXbzwYF6QETUF3SErp9eYORGJZQhZQLXRrBgHHdwgZkQmBdd6Oq1mnJsu6AEtIOmhSfdfwefs0YqonDq1o&disp=emb](https://mail.google.com/mail/u/0?ui=2&ik=91531500aa&attid=0.13&permmsgid=msg-f:1666654586044621034&th=17212584047720ea&view=fimg&sz=s0-l75-ft&attbid=ANGjdJ9Fl6uwP0d1xS7K7Vs_MhhP7rWm3LNy2W9taJRedwXbzwYF6QETUF3SErp9eYORGJZQhZQLXRrBgHHdwgZkQmBdd6Oq1mnJsu6AEtIOmhSfdfwefs0YqonDq1o&disp=emb)

Hierarachical Softmax의 경우 subsampling을 적용했을 때와 그렇지 않을 때의 성능 차이가 매우 컸다. 이는 특정한 경우에 subsampling이 속도 뿐만 아니라 성능도 크게 향상 시켜준다는 것을 의미한다. 또한 subsampling 여부에 관계없이 NEG-5 모델이 NEG-15 모델에 비해 상대적으로 성능이 높게 나왔다.

성능을 높이기 위하여 330억개의 단어가 있는 데이터셋으로 1000차원의 HS모델을 학습시켰을 때, 정확도가 72% 수준 달성하였다. 데이터셋의 사이즈를 60억단어 수준으로 낮추자 정확도가 66%까지 떨어졌다. **구문 학습에서는 데이터셋의 크기가 매우 중요하다.**

## **Additive Compositionality**

실험을 통해 아래와 같이 구문 학습이 가능하다는 것을 확인하였다.

[https://mail.google.com/mail/u/0?ui=2&ik=91531500aa&attid=0.14&permmsgid=msg-f:1666654586044621034&th=17212584047720ea&view=fimg&sz=s0-l75-ft&attbid=ANGjdJ_LLrU-09XYENAoQAwxW0fQvb3nRNXBJTGlkdzEXDS2ROwjg0hjl3tg5nRlcuQG7m8B6-1LiGJJ00PTjiCzGlmJ9unnNNKuGPyb8lQS5BmzQimpcDXN0s7wrIk&disp=emb](https://mail.google.com/mail/u/0?ui=2&ik=91531500aa&attid=0.14&permmsgid=msg-f:1666654586044621034&th=17212584047720ea&view=fimg&sz=s0-l75-ft&attbid=ANGjdJ_LLrU-09XYENAoQAwxW0fQvb3nRNXBJTGlkdzEXDS2ROwjg0hjl3tg5nRlcuQG7m8B6-1LiGJJ00PTjiCzGlmJ9unnNNKuGPyb8lQS5BmzQimpcDXN0s7wrIk&disp=emb)
