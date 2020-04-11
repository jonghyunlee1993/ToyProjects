# Receipt OCR

---

1. 목표

- 영수증 사진을 찍으면 해당 영수증에서 텍스트를 추출하는 코드를 구현해보자



2. 진행 방식

- BGR 이미지를 gray sacle로 변경
- 노이즈 줄이기 위해 이미지에 bilateral filtering 적용
  - `cv2.bilateralFilter`
  - 물론 gaussian filtering도 가능
- 이후 adaptive thresholding을 이용해 이미지에서 적합한 threshold를 판단하고 이를 이용해 이미지 이진화 + median blurring
  - `cv2.adaptiveThreshold`
  - `cv2.medianBlur`
- 경계 판단을 돕기 위해 경계선 부위에 검은색 경계 추가
  -  `cv2.copyMakeBorder`
- Canny edge 검출 실시하여 후보 edge 판별
  - cv2.Canny
- 찾은 edge를 이용하여 contour를 찾아 영수증의 경계를 획득
  - `cv2.findContours`
- 이후 prespective transform하여 관심 영역만 crop
  - `cv2.warpPerspective`
- 최종적으로 OCR은 pytesseract 라이브러리 이용
  - tesseract라는 툴의 python wrapper
  - `pttesseract.image_to_string(...)`



3. 향후 진행 사항

- 글자 인식 성능이 너무 낮다
- 영상의 품질이 좋지 않을 때에 대비하는 방법을 추가하자
- 영수증에서 원하는 품목, 가격의 정보만을 어떻게 가져올 것인가?