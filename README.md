# Yoga-Pose-detection

### Model Architecture for Yoga Pose Detection & Correction

#### 1. Pose Detection Layer (MediaPipe)
The primary task of the pose detection layer is to extract human pose landmarks from images or videos. MediaPipe is used to detect 33 keypoints that represent key joints of the human body, such as the shoulders, hips, and knees. Each keypoint provides normalized (x, y, z) coordinates, along with a visibility score indicating the confidence in the detection. The output of this layer is a flattened vector of 99 features, derived from the 33 keypoints and their associated x, y, and z values.


#### 2. Feature Engineering
After obtaining the raw pose data from MediaPipe, feature engineering refines this data to focus on the most relevant aspects for pose classification. 

- **Normalization**: The coordinates of the landmarks are normalized to eliminate variations caused by factors like camera angle or body size, ensuring consistency across different inputs.
  
- **Angle Calculation**: To better represent the body’s pose structure, angles between key joints are computed (e.g., the elbow angle, knee angle). These angles provide more meaningful information for classification, as they are critical for determining correct posture.

The output of this step is a processed feature vector that serves as the input for the machine learning model.


#### 3. Classification Model
The classification model takes the processed feature vector and uses it to classify the pose into a predefined category, such as Tree Pose, Warrior Pose, or Downward Dog Pose. 

- **Algorithm**: A Random Forest Classifier is chosen for its simplicity, robustness, and interpretability, making it an ideal choice for this proof of concept.
  
- **Hyperparameters**: 
  - `n_estimators`: 100 trees to balance between accuracy and efficiency.
  - `max_depth`: A limitation on tree depth to prevent overfitting and maintain generalizability.

The output of the classification model is the predicted pose class.


#### 4. Feedback Mechanism
Once the pose is classified, the feedback mechanism compares the user’s pose with an ideal reference pose stored in a database. This comparison identifies any deviations in alignment and provides corrective suggestions. 

- **Deviation Highlighting**: If the user’s pose deviates from the ideal reference, feedback such as “Raise your left arm higher” or “Straighten your back” is generated.

- **Real-time Feedback**: This feedback is displayed to the user in real time, allowing them to make immediate adjustments to their posture.



### Decision-Making Process

#### 1. Pose Detection
The system first extracts the keypoints of the user’s pose using MediaPipe’s pose detection model. These keypoints form the basis for further analysis.

#### 2. Feature Vector Construction
The raw data from MediaPipe is processed by normalizing the coordinates and calculating joint angles, resulting in a feature vector that is suitable for machine learning input.

#### 3. Pose Classification
The feature vector is fed into the Random Forest classifier, which predicts the pose category. This classification helps in identifying the pose being performed by the user.

#### 4. Feedback Generation
Once the pose is classified, the system compares it to the ideal reference pose. The differences are quantified, and actionable feedback is generated to help the user correct their alignment.


### Optimizations for Accuracy, Efficiency, and Real-Time Use

#### 1. Accuracy Optimizations
- **Feature Selection**: The system focuses on key features such as joint angles and relative distances between keypoints. This reduces the influence of less important or redundant features.
  
- **Model Choice**: While a Random Forest classifier is used in the proof of concept, a more advanced model like a CNN or LSTM could be employed for greater accuracy. These models can capture temporal and spatial dependencies in the data.
  
- **Data Augmentation**: To simulate real-world variations in poses, techniques such as adding synthetic noise, rotating, or flipping the poses can be used to enhance the dataset.

- **Hyperparameter Tuning**: Techniques like grid search or random search can be used to find the optimal settings for the classifier, improving its performance.


#### 2. Efficiency Optimizations
- **Model Simplification**: For the proof of concept, a lightweight Random Forest classifier is used. Reducing the tree depth or the number of trees can speed up the model's inference time, which is important for real-time performance.

- **Preprocessing**: To speed up the system, unnecessary features such as the z-coordinates can be excluded, especially if they are not needed for classification. This reduces the computational load.

- **Data Normalization**: Normalizing the input data upfront ensures that each frame processed by the model is consistent, eliminating any need for additional processing during runtime.

