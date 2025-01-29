🧠 Beyin Tümörü Segmentasyonu (U-Net) 
📌 Özellikler
Veri Seti: .mat formatındaki MRI görüntüleri ve tümör maskeleri kullanılır. Verilere bu link üzerinden erişim sağlayabilirsiniz  : https://figshare.com/articles/dataset/brain_tumor_dataset/1512427/8
Model Mimarisi: İyileştirilmiş U-Net
Bu model, beyin tümörü segmentasyonu için U-Net mimarisini temel alır. U-Net, özellikle tıbbi görüntü segmentasyonu için geliştirilmiş ve çok etkili sonuçlar veren bir derin öğrenme mimarisidir. Modelin her iki ana bileşeni olan Encoder (Aşağı örnekleme) ve Decoder (Yukarı örnekleme) katmanları, yüksek doğruluklu segmentasyon sonuçları elde etmek için birlikte çalışır.
1. Encoder 
Encoder, giriş görüntüsündeki düşük seviyeli özellikleri çıkararak, görüntünün her seviyesindeki özellikleri analiz eder. Bu süreçte konvolüsyon (Conv2D) ve max-pooling işlemleri kullanılır. Encoder katmanları, görüntüyü daha küçük boyutlara indirger ve her katman bir önceki katmandan daha derin özellikler çıkarır. Aşağıdaki adımlarla encoder kısmı yapılandırılmıştır:
Conv2D katmanları, her bir konvolüsyonel filtre için ReLU aktivasyon fonksiyonu ile çalışarak düşük seviyeli görsel özellikler çıkarır (kenarlar, dokular vb.).
Her konvolüsyon işleminden sonra MaxPooling2D katmanları, her özellik haritasını daha küçük boyutlara indirger, böylece model daha soyut özellikleri öğrenir.
2. Bottleneck 
Encoder bölümünün en derin katmanı olan bottleneck, daha yüksek seviyedeki özelliklerin çıkarıldığı, görüntüdeki en fazla soyutlamanın yapıldığı katmandır. Bu katman, çok daha fazla filtre içerir ve modelin öğrenmesi gereken daha fazla parametreye sahip olur.
Burada, Conv2D katmanları, özelliklerin daha soyut bir şekilde çıkarılmasına olanak sağlar. Bu katmanlar genellikle daha fazla sayıda nöron içerir ve modelin en güçlü temsilini öğrenmesine yardımcı olur.
3. Decoder 
Decoder, encoder tarafından çıkarılan özellikleri kullanarak görüntünün çözünürlüğünü tekrar eski haline getirmeyi amaçlar. Decoder'deki Conv2DTranspose katmanları, up-sampling işlemi gerçekleştirerek görüntünün boyutlarını yeniden artırır. Bu katmanlar, her encoder katmanına karşılık gelen ve yüksek çözünürlüklü özellikler içeren çıktılarla birleşir.
Concatenate işlemi ile her decoder katmanına, karşılık gelen encoder katmanından gelen özellikler eklenir. Bu, modelin daha detaylı ve doğru segmentasyon yapabilmesine olanak tanır.
Decoder, her bir aşamada daha düşük çözünürlükteki özellik haritalarını yeniden inşa eder.
Görüntü İşleme: Model, MRI görüntülerini 256x256 piksel boyutuna yeniden boyutlandırarak işler. Görüntüler ve maskeler [0, 1] aralığında normalize edilmiştir.


🧠 Brain Tumor Segmentation (U-Net)
📌 Features
Dataset: The dataset consists of MRI images and tumor masks in .mat format. You can access the data through this link: https://figshare.com/articles/dataset/brain_tumor_dataset/1512427/8
Model Architecture: Improved U-Net
This model is based on the U-Net architecture for brain tumor segmentation. U-Net is a deep learning architecture specifically developed for medical image segmentation and has shown very effective results. The two main components of the model, the Encoder (Downsampling) and Decoder (Upsampling) layers, work together to achieve high-accuracy segmentation results.
1. Encoder
The encoder extracts low-level features from the input image, analyzing features at each level of the image. Convolution (Conv2D) and max-pooling operations are used in this process. Encoder layers reduce the image size, and each layer extracts deeper features than the previous one. The encoder is structured with the following steps:
Conv2D layers extract low-level visual features (edges, textures, etc.) using the ReLU activation function for each convolutional filter.
After each convolution operation, MaxPooling2D layers reduce the size of each feature map, allowing the model to learn more abstract features.
2. Bottleneck
The bottleneck, the deepest layer of the encoder section, extracts higher-level features and performs the most abstraction in the image. This layer contains many more filters and has more parameters for the model to learn.
Here, Conv2D layers allow the extraction of more abstract features. These layers generally contain a greater number of neurons and help the model learn the most powerful representation.
3. Decoder
The decoder aims to restore the image’s resolution using the features extracted by the encoder. Conv2DTranspose layers in the decoder perform upsampling to increase the image's dimensions. These layers merge with outputs containing high-resolution features corresponding to each encoder layer.
Through Concatenate operations, features from the corresponding encoder layers are added to each decoder layer. This allows the model to perform more detailed and accurate segmentation.
The decoder reconstructs feature maps at lower resolutions in each stage.
Image Processing
The model processes MRI images by resizing them to 256x256 pixels. The images and masks are normalized within the range [0, 1].
