Approach:
Two separate tasks: 
1. Segmentation
2. Classification

Segmentation:

Model Selection:
The GTEA gaze part data set is purely based on cooking hand segmentation. Hence, I decided to stick to this data set for the first experiment. After reading through the segmentation models library documentation, I decided to go with a UNET approach using MobilenetV2 trained weights. I choose this particular model because it performs well on small dataset as it has less parameters with decent accuracy. 

Augmentation:
For augmentation I went with the Albumentation library. It was used in the segmentation model sample example hence, I decided to stick with it. I used horizontal flip, rotate, scale, blurring, noise induction and random cropping Augmentation techniques.

Training:
Divided the data in the ratio of 80% train, 10% validation, 10% test. Used Diceloss and Adam optimizer. Trained model had an iou score of 0.8746 on test data.

Classification:

Data Collection and Preprocessing:
We have to classify as stirring or adding ingredients. But what about the frames where none of the action was performed? Hence I decided to have 3 classes of data stirring, adding ingredients and unknown.
Coming to data collection as there was no dataset available I decided to crawl youtube videos. I found a 32 mins long video on one pot cooking. I downloaded the video using youtube-dl and wrote a small tool on the jupyter notebook using ipywidgets. I sampled the video at 2 fps. It shows the images one by one and asks you to select the class. Based on your selection, it dumps the frame in that particular class folder. There are in total 3900 frames approx. I annotated around 1200 frames myself and used it for training. Below is a snippet of the tool.

Model Selection:
I used a pretrained model weights of mobilenetv2 in tensorflow 2.2 for initialization and retrained the model for this use case. I added a dense layer of 128 neurons with dropout of 0.3 followed by a dense layer of 3 neurons. I used Focal loss to compensate for imbalance data.

Model Accuracy:
Training accuracy of the model is 71% while validation accuracy is 67%. When validated on test data the precision is 0.71 while recall is 0.51 with a F1 score of 0.51. Accuracy of prediction on test data is 63%.

Inference:
There are two inference notebooks, one for inferencing on an image and other for inferencing on a video. For video inference video is segmented at 10 fps.

Suggested Improvements:
1. Currently, 2 different models are trained one for classification and other for segmentation. We can train a model for classification and use it as a freezed encoder in UNET for segmentation.
2. For data collection, I have used very similar videos. This may have introduced a bias. If the model has to work on varied cooking videos data has to come from a varied environment/background.
3. For hand segmentation I have used the GTEA gaze dataset. We can add an EgoHand data set if we want to improve the accuracy.
4. One of the drawbacks is it is colour baised. For this we can apply different color space transforms in augmentation.
