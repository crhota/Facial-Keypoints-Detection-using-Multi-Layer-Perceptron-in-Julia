<img src="https://zenodo.org/badge/DOI/10.5281/zenodo.3242066.svg">

# Facial-Keypoints-Detection-using-Multi-Layer-Perceptron-with-Kaggle-Dataset-in-Julia
This project contains code and explanation behind the code for detecting 15# facial keypoints for a given face image (Kaggle dataset) using Multi Layer Perceptron in Julia.

## 1. Project Objective
We need to detect the x,y coordinates of 15 keypoints of a given image of a face. There are 15 keypoints that need to be detected. Hence, 15x2 = 30 coordinates.Face image data is available as 9216 pixels corresponding to a 96x96 image.

We have training data for various face images and their respective keypoint coordinates. We need to train the model using this training data. As the input and output values are known, model will undergo thro **supervised machine learning**. Output here are keypoints where we need to predict the x,y coordinates - these are not various classes that we need to predict, rather  specific values that needs to be predicted. Hence, this is type of **regression problem**.

Intent of this project is to keep it simple and get a hands on experience of the elements starting from getting the data to predicting the output. Additional design of experiments for optimization, identified during the project has been summarized in the "**Further Work**" section.

Refer the below figure which has a visual representation of the project objective:
<img src="images/ProjectObjective.JPG">

## 2. Gratitude
My sincere gratitude to my teachers for sharing the knowledge on Machine Learning and Julia:
- **Anandi Giridharan**, Principal Research Scientist, Electrical Communication Engg., Indian Institute of Science,Bengaluru, India
- **Dr. Vijaya Kumar B.P**, Professor & Head, Information Science & Engg., MS Ramaiah Institute of Technology, Bengaluru, India
- **Abhijith Chandraprabhu**, Application Engineer, Julia Computing, Bengaluru, India

I sincerely thank [**Julia Slack Community**](https://julialang.slack.com) and **my batchmates of Jan-May'19 Computational Machine Laerning Course at CCE, IISc** for clarifying various doubts of mine which helped a lot at critical junctures of my learning.  

I am indebted to **my family** for supporting me in my quest to learn new things.

## 3. Getting Started
**Tools Involved**
* Julia v1.0.3
* Jupyter

**Folder/Files Involved**
* Folder: code 
  * contains Jupyter Notebook: FP_vX.XX.ipynb
    * Above Jupyter Notebook refers to following dataset files: training.csv, test.csv
    * Link to download the dataset files: [Kaggle](https://www.kaggle.com/c/facial-keypoints-detection/data)
* Folder: images
  * contains image files 
    * ProjectObjective.JPG: shows visual representation of the project objective
    * MLP.JPG: shows visual representation of the model (multi layer perceptron network)

**Setting up the Project to run**
* Say, we place the notebook in a folder called "basefolder"
* We need to save the two dataset files at ~\basefolder\data\
* We then need to run the notebook in Jupyter
* Note: At cold boot, re-run of the whole notebook takes ~4mins to complete run till last statement.

## 4. Description of Datasets
**Training Data** <br>
- File Name/Type: **training.csv** <br>
- Each row corresponds to an instance. There are **7049 rows**. There has **31 columns**. <br>
- **Column 1 to 30** correspond to the x,y coordinates of the 15 keypoints. <br>
- **Column 31** has the face image data. There are **9216 pixel data**. Each pixel data is in the **range [0,255]**<br>

**Test Data** <br>
- File Name/Type: **test.csv** <br>
- Each row corresponds to an instance. There are **1783 rows**. There are **2 columns**. <br>
- Column 1 corresponds to image ID. <br>
- **Column 2** has the face image data. There are **9216 pixel data**. Each pixel data is in the **range [0,255]** <br>

## 5. Algorithm
Here is an high level explanation of the steps followed in building the model - key steps are listed below:
- Data Representaion <br>
- Network Topology <br>
- Network Parameters <br>
- Training <br>
- Validation <br>

Refer the below figure which has a visual representation of the model (multi layer perceptron network):
<img src="images/MLP.JPG">

### 5.1. Network Topology
CNN is recommended for this kind of applications. Intent currently is to build a simple end-to-end solution to so that I have understanding of the machine learning as well as Julia. Hence, I am chosing to **Multi-Layer Perceptron** as the network topology. ...**Further Work 2(a)**

### 5.2. Network Parameters
(9216 Pixel Image) **Input** -> **Model** -> **Output** (30 coordinates of the keypoints) <br>

**Number of Layers / Neurons**
- We need to have an input layer with 9216 neurons. <br>
- We need to have an output layer with 30 neurons. <br>
- To keep it simple, we are having 1 hidden layer with 100 neurons. ...**Further Work 3(a),3(b)** <br>

**Activation Function** <br>
- Input to Hidden Layer
  - We need non-linearity to be there for the input to the hidden layer. 
  - Input Values are pixel data and Output Values are keypoint coordinates. 
  - As both of there are non-negative, chosing the activation function as **ReLU**. f(x) = { 0 for x<0 ; x for x>=0 }
- Hidden to Output Layer
  - Output of the Hidden layer directly predicts the coordinates.
  - We do not wish to modify it in anyway. Hence, chosing the activation function as **Identity**. f(x) = x (for all x)

### 5.3. Data Representation
**Training Data - Input**
* We need to extract the Image Pixel Data from Column 31 for each instance(row).
* Image pixel data range is [0,255]. We need to divide each value by 255 to get the values in [0,1]
* We need to have the pixel data in a **(9216,1) column vector** (array format) - to match to the network input layer
* We need to have multiple above arrays corresponding to various instances in an array

**Training Data - Output**
* We need to extract the 30 coordinates for the 15 keypoints from Column 1-30 for each instance(row).
* Coordinate values are in the range[0,95]. 
* We need to have the coordinates in a **(30,1) column vector** (array format) - to match to the network output layer.
* We need to have multiple above arrays corresponding to various instances in an array

**Training Data - Number of Instances**
* **Total Instances Available = 7049.**
* We have instances (rows) with missing information for few of the keypoints coordinates (columns).
* To keep it simple, for now, **considering the instances (2140) that do not have any missing information. ...Further Work 4(a)**

**Training v/s Validation Data - Number of Instances**
* Considering the breakup as follows: 
  * **first 80% of the 2140 instances**: 1 to 1712 instances as training data ...**Further Work 4(e)**
  * **remaining instances**: 1713 to 2140 instances as validation data ...**Further Work 5(a)**

**Training Data - Input,Output**
* We will provide input and output together as data for the model to train.
  * **1712 number of (9216,1) column vector as Input** 
  * **1712 number of (30,1) column vector as Output**

### 5.4. Training
**Learning Rate** <br>
- We need to take small steps during learning so that we do not miss the minima.
- Hence, keeping the **learning rate = 0.01** ...**Further Work 4(b)** 

**Momentum** <br>
- If the learning rate is too small, there will be small changes to the model internal paramters (weights/bias). Model will learn very slowly (takes more time).
- If the learning rate is too high, there will large changes to the model internal parameters. Model might oscillate and thereby become unstable.
- Using Momentum ensures that we have a faster descent during steady slope and get stability when the slope is oscillating.
- Keepig the **momentum = 0.09** as of now. ...**Further Work 4(c)**

**Optimizer** <gr>
- Chosen **Nestorov** as the optimizer for training to utilize both Learning Rate and Momentum.
    
**Loss Function** <gr>
- Learning rule applied is **Error Correction Learning**.
- Goal is to minimize the error between the desired output and predicted output.
- Chosen **Flux.mse** as the loss function.
    
**Epochs** <gr>
- Considered **1 epoch** for now ...**Further Work 4(d)**

### 5.5. Validation
* Predicted v/s Expected Keypoint Location (Euclidean Distance)
    - Mean of above euclidean distance for all validation instances for all keypoints - summary below:
        - 8 keypoints: Mean < ~4.5
        - 7 keypoints: ~5.3 < Mean < ~6.5 ...**Further Work 4(e)**
    * Refer figure below:
      <img src="images/ModelValidation.JPG">
* Currently I manually validated the data by passing few test samples to the trained model and then plotting the predicted keypoints onto the face image. ...**Further Work 5(a)**

## 6. Test Data Results
Note: 3 center keypoints referred here are nose_tip, mouth_center_top_lip, mouth_center_bottom_lip <br>
- Passed the 1st test instance to trained model and plotted the predicted keypoints onto the test face image.<br>
    - Key Observation: 3 center keypoints are more away from where they should be. <br>
- Calculated Mean of Euclidean Distance b/n "Test Keypoint" and "Mean of Training Keypoint" <br>
    - Key Observation: 3 center keypoints are more away from mean.<br>
        - 12 Keypoints: Mean < ~1.7 <br>
        - 3 Keypoints: ~2.2 < Mean < ~3.9 <br?
    * Refer figure below:
      <img src="images/TestError.JPG">
- Accuracy of 3 center keypoints prediction needs to be improved ...**Further Work 1(a), 4(a), 4(e)**

## 7. Conclusion
Key Observations <br>
- For Training Data: we have 8keypoints more away from expected value. For Test Data: we have 3 keypoints more away from mean.=> Model training error is higher (consistency), however, test error is lesser (generalization).<br>
- For All Test Data: 3 center keypoints are more away from mean. For Test Data1 (visual inspection of plot): 3 center keypoints are more away from where they should be. <br><br>
**Conclusion: Model is generalizing well, however, more test error is seen for the center keypoints which needs to be improved.**

## 8. Further Work
Based on the insights from working on the project, here are a list of future work that I plan to do:
1. Data Representation
   * (a) Try **data augmentation** e.g. flipping the available face images to increase the variety of training data
   * (b) Check which all code sections additionally needs to be **run on gpu** to improve performance e.g. loading the data from the 
         .csv files to dataframes
2. Network Topology
   * (a) Try **CNN** as the Network Topology
3. Network Parameters
   * (a) Try with **more neurons in hidden layer** for the Multi-Layer Perceptron (MLP)
   * (b) Try with **more hidden layers** for the Multi-Layer Perceptron (MLP)
4. Training
   * (a) **Increase Training Data Size** - We have currently used the training/validation data instances (2140) which have information 
         about all 15 keypoints. There are other instances (7049-2140 = 4909) which have keypoints information for few keypoints and do 
         not have for other keypoints. We can utilize these training instances to train the model for the respective keypoints whose 
         information is available. 
   * (b) Try **different values of learning rate**
   * (c) Try **different values of momentum**
   * (d) Try **different values of epochs**
   * (e) **Maximize Information Content** - review the training data and validation results and pick up those samples that are radically 
         different than others.
5. Validation
   * (a) Use **k-cross validation** (split the data into training,validation and perform k times of training & validation)

## 9. References
* Dataset and associated knowledge base for the objective: [Kaggle](https://www.kaggle.com/c/facial-keypoints-detection/data)
* "Intro to Julia" and "Intro to ML" tutorials: [JuliaComputing GitHub](https://github.com/JuliaComputing/JuliaBoxTutorials)
* Knowledge Base on Julia/ML: [Stackoverflow](https://stackoverflow.com/)
* Knowledge Base on Julia: [Julialang Discourse](https://discourse.julialang.org/)
