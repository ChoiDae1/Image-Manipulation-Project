# StyleGAN_Project
**StyleGAN**을 활용해 사용자가 원하는 사진에 대한 **latent vector**를 찾고, **Interpolation** 분석. 

나아가 **StyleCLIP**을 통해 사용자가 입력한 텍스트를 반영한 이미지를 만들어 내도록 **latent vector 업데이트**.
</br></br>
## STEP1: 특정 사진에 대한 Latent vector 찾기 (Image2StyleGAN)
**구현 코드:** [link](https://github.com/ChoiDae1/StyleGAN_Project/blob/master/Image2StyleGAN.ipynb)</br>
**참고 논문:** [Image2StyleGAN: How to Embed Images Into the StyleGAN Latent Space?](https://arxiv.org/abs/1904.03189)</br>
**참고 코드:** [link](https://github.com/ndb796/PyTorch-StyleGAN-Face-Editting)</br>
**방법:** **Pretrained StyleGAN, VGG16**을 이용해 latent vector가 만들어내는 이미지와 원본 이미지와의 **loss**를 계산 -> **latent vector 업데이트**</br></br>
**Image2StyleGAN**은 _이미지를 representation하는_ **latent vector**를 **Pretrained StyleGAN**을 사용해 찾는 것을 말한다.</br>
이러한 과정을 통해 특정 이미지에 대한 latent vector를 찾게되면, 이를 조작해 **이미지의 Style을 바꾸거나, 다른 이미지로 전환**할 수 있음.</br></br>
구글에서 많은 연예인들의 사진들을 찾을 수 있었고, 결과적으로 다양한 사진에 대한 latent vector을 얻을 수 있었음.</br>
Latent vector들 중 상당수는 입력 이미지를 잘 represent했지만, 일부는 그렇지 못했음.</br></br>
**Example**) **왼쪽**은 구글에서 구한 **원본 이미지**고, **오른쪽**은 Image2StyleGAN 방법론을 통해 찾은 **latent vector를 통해 만들어진 이미지**</br></br>
**<이미지를 잘 represent하는 Latent vector 찾은 경우>**</br>
<img width="20%" src="https://user-images.githubusercontent.com/95220313/155799611-4963c377-dc15-483f-a2ec-3ea4db2d4626.jpg"/>
<img width="19.7%" src="https://user-images.githubusercontent.com/95220313/155800508-06980c44-ca2e-4dcc-bbea-3a853b70965c.png"/></br></br></br>
**<이미지를 잘 represent하지  _못하는_ Latent vector 찾은 경우>**</br>
<img width="20%" src="https://user-images.githubusercontent.com/95220313/155802018-8956de1f-170f-43f1-af9f-dd8b57aa076a.jpg"/>
<img width="20%" src="https://user-images.githubusercontent.com/95220313/155802781-5a0b04f0-c173-4b34-9b4e-576ae37e66fe.png"/></br></br>
많은 수의 이미지를 통해 여러번 실험을 진행함, </br>
결과적으로 representation이 좋은 latent vector를 가지고 있는 원본 이미지는 **_다음과 같은 특징들_** 을 가지고 있다는 것을 확인함. </br>
- 보통 **서양인**임. (대체로 한국인 이미지보다 더 좋은 latent vector를 가졌음.) 
- input으로 들어가는 이미지에 **noise가 거의 없음, 화질 좋음.**
- 사진에 **얼굴이 차지하는 비율**이 거의 **80퍼~90퍼**(얼굴외에 손이나 기타 다른 부위 혹은 배경들이 거의 보이지 않음)
- 옆모습이나 측면보다 **정면샷**으로 찍은 얼굴이 더 잘 나옴.

## STEP2: Latent Interpolation
**구현 코드:** [link](https://github.com/ChoiDae1/StyleGAN_Project/blob/master/Latent_vector_Analysis.ipynb)</br>
**참고 코드:** [link](https://github.com/zaidbhat1234/Image2StyleGAN/blob/main/Image2Style_Implementation.ipynb)</br>
**방법:** **STEP1**에서 두 이미지에 각각 대응하는 **latent vector**를 찾음 -> 계수합이 1인 **linear combination**으로 표현 -> **계수값에 변화**를 조금씩 주면서, 그에 따라 변하는 **이미지 변화** 확인 </br></br>
**StyleGAN**은 Mapper Network 등을 사용함으로써 모델 자체의 latent vector(W)가 이미지의 특징들을 **disentangle**하게 담아내도록 학습함.</br>
이 말은 즉, _latent space_ 상에서 비슷한 거리에 있는 latent vector들은 서로 비슷한 이미지의 특징을 담아내고, 멀리있는 latent vector들은 서로 구별되는 특징을 가질 수 있음.</br>
**Latent Interpolation**은 이러한 **latent space의 특징**을 이용한 것임.</br></br>
**Example**) **STEP1**에서 이미지를 **잘 represent하는 latent vector**를 통해 **Latent Interpolation**을 진행함.</br></br>
**<로제의 latent vector와 제니의 latent vector를 사용해 진행한 결과>**</br>
![image](https://user-images.githubusercontent.com/95220313/155807104-61dc46f0-bee7-4a8f-8843-a74e98037686.png)</br></br></br>
**<오바마 대통령의 latent vector와 조커의 latent vector를 사용해 진행한 결과>**</br>
<img width="20%" src="https://user-images.githubusercontent.com/95220313/155807476-87ed3c31-7bde-4295-ab2b-a6886bb260a2.gif"/>

## STEP3: StyleCLIP
**구현 코드:** [link](https://github.com/ChoiDae1/StyleGAN_Project/blob/master/StyleCLIP.ipynb)</br>
**참고 논문:** [StyleCLIP: Text-Driven Manipulation of StyleGAN Imagery](https://arxiv.org/abs/2103.17249)</br>
**참고 코드:** [link](https://github.com/ndb796/StyleCLIP-Tutorial/blob/main/StyleCLIP_Latent_Optimization.ipynb)</br> 
**방법:** **STEP1**에서 구한 **latent vector가 만들어내는 이미지**와 사용자가 넣어주는 **text**를 **CLIP**에 넣어서 **loss**계산 -> **latent vector 업데이트**</br></br>
**StyleCLIP**은 **StyleGAN**과 **CLIP**을 같이 쓴 모델임. CLIP은 **이미지**와 **텍스트**를 동시에 받아서 둘을 각각 인코더를 거친 후 같은 임베딩 스페이스상에서 거리를 측정하는 도구라 할 수 있다.
이러한 CLIP을 이용하면, 최종적으로 **이미지와 텍스트사이**의 일종의 _**유사도**_ 를 계산할 수 있고, 이를 활용한 loss를 **CLIP loss**라 함. StyleCLIP에서는 이를 사용해 최종적으로 사용자가 넣어준 
텍스트를 잘 표현하는 이미지를 만들어 낼 수 있도록 latent vector를 업데이트 함. 물론 기존 이미지의 전체적인 apperance는 유지함. 

StyleCLIP 논문에서는 구체적으로 **3가지의 방법**을 제시하는데, 여기서는 이 중 가장 직관적이면서도 쉬운 "**Latent Optimization**"을 구현했음. </br>
실험결과 대체로 어느정도 text를 잘 반영하는 이미지를 생성가능했지만, 일부 latent vector에서는 이미지 왜곡이 생기면서 잘 작동하지 않는 것을 확인할 수 있었음.</br></br>
**Example**) **STEP1**에서 이미지를 **잘 represent하는 latent vector**를 활용해 실험 진행함.</br>
### Good case</br>
**<latent vector: Jablonski, Text Prompt: Really angry face>**</br>
<img width="35%" src="https://user-images.githubusercontent.com/95220313/155854429-a6f6bcb2-a17a-42f1-8d51-2979fec92e0a.png"/></br></br></br>
**<latent vector: 엠마스톤, Text Prompt: Smile face>**</br>
<img width="35%" src="https://user-images.githubusercontent.com/95220313/155854716-05c42f4b-1943-48f3-a989-9acb66645b63.jpg"/></br></br>

### Bad case</br>
**<latent vector: 로제, Text Prompt: Really angry face>**</br>
<img width="35%" src="https://user-images.githubusercontent.com/95220313/155855229-2f303217-f6f3-4584-a9c2-2dcfadb7042a.jpg"/></br></br>
**<latent vector: 제니, Text Prompt: Smile face>**</br>
<img width="35%" src="https://user-images.githubusercontent.com/95220313/155854772-e6a28700-1416-4bce-8c46-eedc5606da66.jpg"/></br></br>
이외에도 **여러가지 latent vector와 Text Prompt 조합**을 통해 실험했고,</br>
원본 이미지를  _아무리 좋은 quality 로 representation_ 하는 **latent vector**를 사용하더라도, **StyleCLIP**에서  _잘 작동하지는 않는_ 사실을 알았음. </br>
이와 더불어 **StyleCLIP Task**에서 역시 **한국인 이미지**보다 **서양인 이미지**에서 _더 잘 작동하는 것_ 을 확인할 수 있었는데, 아마도 StyleGAN을 **pretrain할 때 사용한
데이터의 bias문제**라고 판단됨.</br>향후 한국인 이미지 관련 프로젝트를 한다면 다른 pretrain dataset을 사용할 필요가 있어보임.(ex: 한국인 face dataset)



