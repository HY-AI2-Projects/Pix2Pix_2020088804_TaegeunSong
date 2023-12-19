
# Pix2Pix_2020088804_TaegeunSong
참고논문 : https://openaccess.thecvf.com/content_cvpr_2017/papers/Isola_Image-To-Image_Translation_With_CVPR_2017_paper.pdf

Pix2Pix는 cGAN 구조를 갖는다, 이미지 조건을 입력받아 이미지를 출력. 입출력 차원이 동일한 아키텍처로 U-Net을 사용한다.
![참고자료1](https://github.com/HY-AI2-Projects/Pix2Pix_2020088804_TaegeunSong/assets/110830754/2935d152-d694-4c42-8491-d1984ef245e7)


# 구현 과정
1. 학습 데이터셋에서 실제 이미지와 그림으로 그린 이미지를 제공한다.
- 그린 이미지는 건물의 외벽/창문/문과 같은 정보들을 각각의 레이블마다 특정 색갈로 칠함으로, 픽셀 단위로 클래스를 구분한다.
![참고자료2](https://github.com/HY-AI2-Projects/Pix2Pix_2020088804_TaegeunSong/assets/110830754/7a9b2ac5-8059-41bb-b6f9-23951a7f2775)


2. 학습 데이터 이미지 크기는 256X256 이미지 두 개를 이어붙인 형태
- 실제 학습 진행 시 오른쪽(실제이미지), 왼쪽(제공되는 그린 이미지)를 나누어서 진행한다.
![참고자료3](https://github.com/HY-AI2-Projects/Pix2Pix_2020088804_TaegeunSong/assets/110830754/6f27da20-7739-4bb9-a7f9-361eb0ee9a33)

3. U-Net
- U-Net 구조는 다음과같이 skip connection을 이용한다.
- 인코더 출력값을 디코더에서도 활용하기에 많은 lw-level information이 입력과 출력 과정에서 공유될 수 있다.
![참고자료4](https://github.com/HY-AI2-Projects/Pix2Pix_2020088804_TaegeunSong/assets/110830754/d7aa7099-b0e4-425e-aa2f-61fd345b1e19)
![코드구현 자료1](https://github.com/HY-AI2-Projects/Pix2Pix_2020088804_TaegeunSong/assets/110830754/e0cf76bd-45d5-469b-b95b-cbb9593b0f37)


4. 생성자
- 생성자를 통해 Down 샘플링을 진행 : 8번의 convolution layer를 거친 후  512x1x1의 출력 값을 형성한다.
- 생성자를 통해 Up 샘플링을 진행 : 이 부분을 디코더라고 볼 수 있다. Skip Connection을 사용하기 때문에, 출력 채널의 크기 x 2가 다음 입력 채널의 크기가 된다.
![생성자](https://github.com/HY-AI2-Projects/Pix2Pix_2020088804_TaegeunSong/assets/110830754/f6719bb5-bb2d-4f80-a118-2340c3f6e13c)


4-1. 생성자2
![생성자2](https://github.com/HY-AI2-Projects/Pix2Pix_2020088804_TaegeunSong/assets/110830754/36fed8c2-3082-4cf1-991d-5b214502d9d0)
- d8: 512 X 1 X 1 즉 인코더 디코더 중앙의 블럭 출력값
- d7: 중앙 전 인코딩 마지막 출력값
- u1의 매개변수: d8은 Up 샘플링 one 블럭을 거쳐서처리가 되고, d7이 그대로 채널 레벨로 연결되어 u1에 담긴다.
- 결과적으로 디코딩까지 수행 후 final블럭을 거쳐 3 X 256 X 256의 입력이미지와 동일한 차원의 출력 이미지를 만든다.

5. 판별자
- 특정 이미지가 진짜인지, 가짜인지 판별하기 위한 용도이다.
- 너비와 높이가 2배씩 감소하면서 채널값은 커지는 형태로 일반적인 convolution 연산을 수행한다.
- 다만, conditional Gan이기에 입력 조건(x) 이미지와 정답 이미지(y)가 같이 들어와야한다.
- 조건 이미지와 실제/변환 이미지 두 개를 입력 받으므로 채널 크기도 2배(rgb(3) *2 = 6)
- 결과적으로 채널 값은 커지면서 너비와 높이는 감소한 feature를 뽑은 후 출력 값을 내보낸다.
![판별자](https://github.com/HY-AI2-Projects/Pix2Pix_2020088804_TaegeunSong/assets/110830754/8e8188e5-a34d-412e-98a9-9dcec806df40)








  

