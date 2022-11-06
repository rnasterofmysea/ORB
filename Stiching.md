```
import cv2, numpy as np

imgL = cv2.imread('img1.jpg')
imgR = cv2.imread('img2.jpg')

hl, wl = imgL.shape[:2]
hr, wr = imgR.shape[:2]

grayL = cv2.cvtColor(imgL, cv2.COLOR_BGR2GRAY)
grayR = cv2.cvtColor(imgL, cv2.COLOR_BGR2GRAY)

orb = cv2.ORB_create()

kp1, des1 = orb.detectAndCompute(imgL, None)
kp2, des2 = orb.detectAndCompute(imgR, None)

matcher = cv2.DescriptorMatcher_create('BruteForce')
matches = matcher.knnMatch(des2, des1, 2)
good_matches = []
for m in matches:
  if len(m) == 2 and m[0].distance < m[1].distance * 0.75:
    good_matches.append((m[0].trainIdx, m[0].queryIdx))
if len(good_matches) > 4:
  pts1 = np.float32([kp1[i].pt for (i, _) in good_matches])
  pts2 = np.float32([kp2[i].pt for (i, _) in good_matches])
  mtrx, status = cv2.findHomography(pts2, pts1, cv2.RANSAC, 4.0)
  panorama = cv2.warpPerspective(imgR, mtrx, (wr + wl, hr))
  panorama[0:hl, 0:wl] = imgL
else:
  panorama = imgL

from google.colab.patches import cv2_imshow      

cv2_imshow(panorama)
cv2.waitKey()
cv2.destroyAllWindows()
```


```
import cv2

# 3장의 영상을 리스트로 묶어서 반복문으로 하나하나 갖고옴
img_names = ['img1.jpg', 'img2.jpg', 'img3.jpg']

# 불러온 영상을 imgs에 저장
imgs = []
for name in img_names:
    img = cv2.imread(name)
    
    if img is None:
        print('Image load failed!')
        
    imgs.append(img)
    
# 객체 생성
stitcher = cv2.Stitcher_create()

# 이미지 스티칭
status, dst = stitcher.stitch(imgs)

if status != cv2.Stitcher_OK:
    print('Stitch failed!')
    
# 결과 영상 저장
#cv2.imwrite('output.jpg', dst)

# 출력 영상이 화면보다 커질 가능성이 있어 WINDOW_NORMAL 지정
cv2.namedWindow('dst', cv2.WINDOW_NORMAL)
# 결과 출력                    
from google.colab.patches import cv2_imshow      

cv2_imshow(dst)
cv2.waitKey()
cv2.destroyAllWindows()
```
