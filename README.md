# A New Wave of Texture Feature: Enhancing Deepfake Detection via Image Waveform

## Introduction
- Deepfake images generated by deep learning techniques have become increasingly sophisticated, rendering traditional detection methods less effective.

- This paper proposes a texture-based deepfake detection method that transforms an image into an Image Waveform by arranging the RGB pixel values column-wise, which is then analyzed using a convolutional neural network (CNN).

- A two-stream CNN architecture is employed, where one stream processes the original image and the other processes the corresponding Image Waveform. The extracted features from both streams are combined to determine whether the input image is real or a deepfake.

- Experiments conducted on the Celeb-DFv2 dataset demonstrate that the proposed method outperforms existing approaches in terms of detection accuracy. The key contributions of this work include the introduction of Image Waveform-based features, the design of a two-stream CNN architecture, and the empirical validation of the method's effectiveness.


## Image Waveform Conversion

![image](https://github.com/user-attachments/assets/01d2b16f-3bc3-419a-b920-78b9420d13dc)

- The input image is a face crop image of 256×256, and the converted image waveform is 256×256, which is the same size.
- The horizontal length of the image waveform corresponds to the horizontal length of the original image, and the vertical length represents the pixel value range (i.e., 0 to 255) of the cropped image.

(a) The crop image is divided into pixel-wise columns, and all RGB pixel values (e.g., column#75 in the above picture) are extracted from each column.

(b) Duplicate pixel values (e.g., 122, 123, 124) among extracted data from each RGB channel are removed.

(c) The remaining pixel values in each RGB channel are mapped 1:1 to the row index of the corresponding channel of the image waveform. In this case, a value of 255 is allocated to the mapped row index and 0 is allocated to the unmapped row.

(d) Finally, RGB channels are combined to finally create Image Waveform.

![image](https://github.com/user-attachments/assets/84e02e83-4b13-45df-99b9-aa89f8d0fa79)

- The figure above shows the visual patterns that appear in the Image Waveform of the original and smoothed images.
- The green box in (b) shows that the edges of the waveform look rough, which reflects the characteristics of the original image.
- On the other hand, the green box in (d) shows that the edges appear smoothly as the target pixel is adjusted to a value similar to that of the surrounding pixels, and the overall pixel value changes uniformly.
- The red box in (b) shows blue and green waves spread widely downwards due to the different colors of the original image.
- Conversely, the red box in (d) shows that the pixel values of that color are uniformly adjusted due to the smoothing process, making them vertically dense.

## Two-Stream based Deepfake Detection

![image](https://github.com/user-attachments/assets/c4bbbc78-6b89-40bc-aa0d-4b08bfa6db30)

### Two-stream Convolutional Neural Networks (CNN)
- The first stream learns features extracted from the original pixel values of the input image, and the second stream learns the texture features obtained from the Image Waveform.
  - **Train stage**
    - The proposed method generates a face cropped image to focus on the face area from the input image and converts it into an image waveform.
    - Each stream extracts a **feature map** from the input image and the corresponding image waveform.
    - Both streams use VGG19 architecture
    - The extracted feature map is combined through bilinear pooling to generate a bilinear pooling vector, which is connected to a fully connected layer.
    - Two-stream CNNs are trained to determine whether the input image is deepfake
  - **Inference stage**
    - After the input images are facial cropped and transformed into ImageWaveform, they are delivered to the trained two-stream CNN to perform deepfake detection.

## Experiments
|Model|ACC(%)|AUC(%)|
|------|---|---|
|Image Only Model|92.86|93.09|
|Our Two-Stream Model|95.95|96.11|




