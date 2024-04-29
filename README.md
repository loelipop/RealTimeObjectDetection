現在是可以直接執行這個app的狀態

要換模型就要到CameraActivity.java修改第77行 objectDetectorClass=new objectDetectorClass(getAssets(),"1.tflite","labelmap.txt",300);成 objectDetectorClass=new objectDetectorClass(getAssets(),"best_float32.tflite","labelmaptraffic.txt",416);

然後要去objectDetectorClass.java修改private ByteBuffer convertBitmapToByteBuffer(Bitmap bitmap) 的 boolean isQuantized = true;變成false(因爲模型是浮點數）
如果沒改就會有java.lang.IllegalArgumentException: Cannot copy to a TensorFlowLite tensor (normalized_input_image_tensor) with 270000 bytes from a Java Buffer with 1080000 bytes.這個error

但我改了就會變成java.lang.IllegalArgumentException: Cannot copy from a TensorFlowLite tensor (Identity) with shape [1, 17, 3549] to a Java object with shape [1, 10, 4].

後來我去改public Mat recognizeImage(Mat mat_image)的
//we create treemap of three array (boxes,score,classes)
float[][][]boxes =new float[1][10][4];
// 10: top 10 object detected
// 4: there coordinate in image
float[][] scores=new float[1][10];
// stores scores of 10 object
float[][] classes=new float[1][10];
// stores class of object

變成

float[][][]boxes =new float[1][17][3549];
float[][] scores=new float[1][17];
float[][] classes=new float[1][17];
error變成java.lang.illegalargumentexception: invalid output tensor index: 1
