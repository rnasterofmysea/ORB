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
