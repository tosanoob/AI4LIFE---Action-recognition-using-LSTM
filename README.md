### - Xây dựng mô hình ứng dụng Mediapipe và LSTM để nhận diện động tác thể hình qua video - ###
Cuộc thi AI4LIFE 2024 - Trường Đại học Bách Khoa - Đại học Đà Nẵng

## Tổ chức dữ liệu ##
Thư mục dữ liệu huấn luyện đầu vào <a href="https://drive.google.com/drive/folders/18mfPELuFfHY4KfwOmPod-z1-AhrKNRrN?usp=drive_link"> ở đây </a> <br>
Thư mục kiểm thử <a href="https://drive.google.com/drive/folders/1nc8jGLMToDnxdOsA5Vh0k0UFepmjZ5JV?usp=drive_link"> ở đây </a> <br>
Dữ liệu huấn luyện được tổ chức ở dạng cây thư mục /Directory/label/video.mp4
</br>
/Directory
</br>
-----/label_1
</br>
----------/video1.mp4
</br>
----------/video2.mp4
</br>
----------/...
</br>
-----/label_2
</br>
----------/video1.mp4
</br>
----------/video2.mp4
</br>
----------/...
## Tiền xử lý ##
Với mỗi video đầu vào, nhóm chúng tôi sử dụng Mediapipe để nhận diện pose trong từng frame hình ảnh. Kết quả nhận diện thành công sẽ trả về một tập các 'keypoint' (x,y,z,visibility) đại diện cho vị trí của các bộ phận cơ thể người trong video. 
<br> Tập hợp các điểm keypoint ứng với từng frame sẽ được lưu lại theo thứ tự thành file .csv cho từng video. Để nhận diện chính xác động tác, nhóm chúng tôi tổ chức các record gồm chuỗi N bộ keypoint liên tiếp, ứng với chuỗi N frame liên tiếp của 1 video. 
<br> Bởi các động tác thể dục thể hình có tính liền mạch, do đó ta cần một chuỗi các frame liên tiếp thay vì chỉ 1 frame để có thể xác định chính xác động tác.
## Mô hình ##
Dưới đây là kiến trúc mô hình cho kết quả cao nhất cho đến hiện tại trên dữ liệu kiểm thử: đạt accuracy 82% và F1_score 0.8 trên từng video.
![image](https://github.com/tosanoob/AI4LIFE---Action-recognition-using-LSTM/assets/89732559/dad98729-32f3-45c3-a91f-25721d0cdc0e)
Mô hình mạng LSTM được xây dựng bằng Keras 3.0.2, Tensorflow 2.16.1
## Nhận xét & phát triển ##
Việc chỉ xét đến yếu tố vị trí khung xương có thể gây nhầm lẫn giữa các động tác có tư thế gần giống nhau. Ví dụ hai động tác lat pulldown và lateral raise có motion giống nhau gần như hoàn toàn, chỉ khác biệt ở xu hướng dùng lực kéo xuống và đẩy lên.
Tuy nhiên vẫn còn đó yếu tố hình ảnh chưa được sử dụng để phân loại. Cụ thể, nhóm định hướng sử dụng kết hợp Conv3D để phân tích chuỗi hình ảnh, và LSTM để phân tích chuỗi keypoint giúp tăng cường độ chính xác phân loại.
