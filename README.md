# Concrete-Prediction Machine Learning

## Project Summary
This project builds a neural network to classify concrete strength into three levels: low, medium, and high, using numerical features from the dataset. The data is preprocessed by encoding the target variable and splitting it into training and testing sets. A baseline model with two hidden layers is initially trained and evaluated. To improve performance, outliers are removed, features are normalized, and a more complex neural network with dropout layers is developed to prevent overfitting. The model's accuracy is evaluated through a classification report, confusion matrix, and training/validation loss plots. Results indicate that the improved model performs well, with accurate predictions and no signs of overfitting or underfitting.

## Code Setup
You can simply download the concrete data in the data folder on this repository named concrete.csv and also download the code in the src folder where the source code is. I used Google Colab to create this code and I had to upload the data into the environment to read it but for other python environments you can do concrete = pd.read_csv('C:/Users/Computer Username/Downloads/concrete.csv').

## Code Overview

### 1.Packages, setting seed, and reading the CSV concrete data
This code imports various libraries and modules for data analysis, preprocessing, and building neural networks. It uses Pandas and NumPy for data manipulation, Seaborn and Matplotlib for visualization, and Scikit-learn for preprocessing and model evaluation. TensorFlow and Keras are included to create and train a neural network, utilizing layers like Dense and Dropout to build the model. It also incorporates functions for handling categorical data and optimizing the training process. Overall, this setup is essential for preparing data and constructing machine learning models effectively.

![image](https://github.com/user-attachments/assets/cd890576-be53-455e-be66-a430192eec24)

np.random.seed(42): This sets the seed for NumPy's random number generator. By specifying a seed value (in this case, 42), you ensure that any random numbers generated by NumPy functions (e.g., for creating random arrays or shuffling data) will be the same each time the code is run. This is useful for consistency in experiments and results.

tensorflow.random.set_seed(42): Similarly, this sets the seed for TensorFlow's random number generation. This ensures that operations involving randomness in TensorFlow (like initializing weights in a neural network) produce the same results each time the code is executed.

![image](https://github.com/user-attachments/assets/12ec7400-431f-4d34-bc51-6bf7d9959a58)

Using the Pandas read_csv function to read in the concrete CSV data into a pandas dataframe named concrete.

![image](https://github.com/user-attachments/assets/aaa9e17f-3413-4c3b-b157-3cab5ade48ac)

### 2.Data information, initial cleaning, and baseline model. 
This output is a summary of a Pandas DataFrame containing 1,030 entries and 9 columns. Each column represents a different feature related to concrete composition and strength, including components like cement, slag, and water, with data types primarily as float64 for continuous variables and int64 for the age column. The Non-Null Count indicates that all entries in each column have valid data (no missing values).

![image](https://github.com/user-attachments/assets/924d5ded-2184-47f7-9aae-096cfe32ccb2)

The first five rows of the dataset represent various concrete mixtures with different compositions and their corresponding compressive strength. Each row includes values for the amounts of cement, slag, ash, water, superplasticizer, coarse aggregate, fine aggregate, age (in days), and strength (in MPa). For instance, the first entry has 540.0 kg of cement, no slag or ash, 162.0 kg of water, and a compressive strength of 79.99 MPa at 28 days of age. The second entry has the same cement and water content but a lower strength of 61.89 MPa. The subsequent rows show different combinations of materials and ages, resulting in varying strengths, indicating how different mixtures affect concrete's compressive strength over time.

![image](https://github.com/user-attachments/assets/9a798f2d-d646-4333-b780-66e620897d1b)

I created a function called strength_level to categorize concrete compressive strength into three distinct levels: 'low', 'medium', and 'high.' This classification is based on specific criteria: strengths below 20 MPa are classified as 'low', those between 20 and 40 MPa are classified as 'medium', and strengths above 40 MPa are classified as 'high'. After defining the function, I applied it to the 'strength' column in my concrete DataFrame. As a result, I generated a new column named 'strength_level,' which holds the corresponding category for each concrete strength value.

![image](https://github.com/user-attachments/assets/9e25ec05-31b9-42e2-8983-058f99c642b7)

In this code, I first prepared the dataset for prediction by creating two separate variables: concrete_x and concrete_y. The concrete_x variable contains all numerical features from the concrete DataFrame, excluding the strength_level and strength columns, while concrete_y holds the target variable, which is the strength_level.

Next, I utilized the LabelEncoder to convert the categorical strength_level variable into a numerical format. After encoding, I applied one-hot encoding to transform the encoded labels into a binary matrix, allowing for more efficient training of machine learning models.

Finally, I split the data into training and testing sets, allocating 70% of the data for training and 30% for testing, using a fixed random seed (42) to ensure reproducibility. This prepares the data for training a classification model on the concrete strength levels.

![image](https://github.com/user-attachments/assets/15f67876-c96e-404f-9068-4ab33b80f3a2)

In this code, I created a neural network model(baseline model) to predict concrete strength levels based on various input features. The model follows a sequential architecture and consists of multiple layers:

Input Layer: The first layer has 128 units and uses the ReLU activation function, which is designed to handle the inputs from the dataset based on the number of features.
Hidden Layers: The model includes two hidden layers: the first has 64 units, and the second has 32 units, both utilizing the ReLU activation function to introduce non-linearity.
Output Layer: The final layer has 3 units, corresponding to the three strength categories ('low', 'medium', 'high'), and employs the softmax activation function to provide probabilities for each class.
The model is compiled using categorical cross-entropy as the loss function, the Adam optimizer for efficient backpropagation, and accuracy as the evaluation metric. After setting up the model, I fit it to the training data without displaying detailed output (using verbose=False).

Finally, I evaluated the model on the test dataset, which resulted in a loss of approximately 5.0228 and an accuracy of about 48.22%. This indicates that the model is currently performing moderately, suggesting that there is room for improvement in predicting the concrete strength levels accurately.

![image](https://github.com/user-attachments/assets/53e38938-a80f-4d61-8afc-39697bb85dba)

### 3.More cleaning


I aimed to visualize the presence of outliers in the 'age' variable of my dataset using a box plot. The box plot provides a clear graphical representation of the data distribution, including key statistics such as the median, quartiles, and the range of the data. I interchanged this with all the variables except for strength and strength_level. 

![image](https://github.com/user-attachments/assets/35c429f3-2769-4204-9b29-97c2bfa2376f)


I created a duplicate of my original dataset named concrete2 to preserve the original data while performing data cleaning. The goal was to remove outliers from the dataset based on a defined criterion: any data points that lie beyond two standard deviations from the mean for each numerical variable, except for the 'strength' and 'strength_level' columns.

I used a loop to iterate through each column in the concrete2 DataFrame. For each numerical variable, I calculated its mean and standard deviation. I then established lower and upper bounds, which were set at two standard deviations below and above the mean, respectively. Finally, I filtered concrete2 to retain only the rows where the values of the current column fell within these bounds. This process effectively removes extreme values from the dataset, helping to ensure that the data used for analysis and modeling is cleaner and less influenced by outliers.

![image](https://github.com/user-attachments/assets/fc54b4e6-7288-49be-b072-93e4993a52ce)


In this code, I prepared my dataset for modeling by first creating a new variable, concrete_x2, which contains the numerical features used for prediction, excluding the 'strength' and 'strength_level' columns. I then initialized a MinMaxScaler from the sklearn.preprocessing module to normalize the data, which is an essential step to ensure that all features are on the same scale.

Using the fit_transform method of the MinMaxScaler, I normalized all the numerical columns in concrete_x2. This transformation scales the values of each feature to a range between 0 and 1, which helps improve the performance and convergence of machine learning algorithms, particularly those sensitive to the scale of input data.

Lastly, I collected the target variable, concrete_y2, which contains the 'strength_level' column that we aim to predict. This preparation sets the stage for further modeling and analysis by ensuring that the features are well-scaled and appropriately organized for training a predictive model.

![image](https://github.com/user-attachments/assets/22db55fe-fde4-4e1a-97b7-e75dfb81d350)

I prepared the target variable for the model by encoding the categorical 'strength_level' variable into a numerical format using LabelEncoder. This transformation is crucial because machine learning algorithms typically require numerical input for training.

After encoding the labels, I applied one-hot encoding to convert the encoded values into a binary matrix representation, which is necessary for models that output probabilities for each class (in this case, 'low', 'medium', and 'high' strength levels). This step effectively creates three binary columns corresponding to the three categories of strength.

Next, I split the dataset into training and testing sets, using 70% of the data for training and 30% for testing. This split is vital for evaluating the model's performance, allowing it to learn from one subset of the data while being tested on a separate, unseen subset to assess its generalization ability. The random_state parameter is set to 42 for reproducibility, ensuring that the same split can be obtained in future runs.

![image](https://github.com/user-attachments/assets/0dd221a9-b6b4-4dba-ab75-acf7edc45eca)

### 4.Neural Network Model

In the first section, I constructed a neural network model named concrete_model2 using the Sequential API from Keras. The model starts with an input layer comprising 256 units and uses the ReLU activation function, which helps in introducing non-linearity. Each layer, including the input layer, is followed by a Dropout layer, which randomly deactivates a percentage of neurons during training (set at 40% for the input layer). This technique helps prevent overfitting by ensuring that the model does not become too reliant on any specific neurons.

Subsequent hidden layers are added, with 128, 64, and 32 units, each utilizing ReLU activation and accompanied by decreasing dropout rates (30%, 20%, and 10%, respectively). This gradual reduction in dropout rates allows the model to learn more while still maintaining some regularization to combat overfitting. Finally, the output layer consists of three units, corresponding to the three classes of the target variable, and uses the softmax activation function to produce probabilities for each class.

![image](https://github.com/user-attachments/assets/69f91774-d950-4354-9eed-8f462cf92c04)

In the second section, I defined the learning process for the model. The loss function used is categorical crossentropy, which is appropriate for multi-class classification problems. The Adam optimizer is employed for its efficiency in adjusting the learning rate during training and its ability to handle sparse gradients. The model's performance is tracked using accuracy as a metric, providing insights into how well the model classifies the data during training and evaluation.

![image](https://github.com/user-attachments/assets/cfa1a308-3893-466f-8353-9ac67518437e)

The third section involves setting up callbacks to enhance training efficiency. An EarlyStopping callback is implemented to halt training if there is no improvement in validation loss over a specified number of epochs (10 in this case), thus preventing unnecessary training. The ReduceLROnPlateau callback is also utilized, which reduces the learning rate if the model shows no improvement over five epochs, allowing the model to refine its learning in later stages.

The model is then fitted to the training data for a maximum of 35 epochs, with a batch size of 16. A validation split of 20% is used to monitor the model's performance on unseen data during training. Upon completion of training, the model is evaluated on the test dataset, yielding a loss of approximately 0.3391 and an accuracy of about 87.61%. These results indicate that the model performs well, with a high accuracy rate in classifying the strength levels of concrete, demonstrating its effectiveness and generalization capabilities.

![image](https://github.com/user-attachments/assets/5a21e93b-71ca-42b1-ab3a-576df6ee32e6)

### 5.Model Evaluation and Final Results

Classification Report: In this section, the model's performance is evaluated using a classification report that provides key metrics: precision, recall, F1-score, and support for each class. After predicting the class labels from the test data, the classification report is generated. The report shows:

High: Precision of 0.93, Recall of 0.83, and F1-score of 0.88 with 83 instances.
Low: Precision of 0.88, Recall of 0.94, and F1-score of 0.91 with 49 instances.
Medium: Precision of 0.83, Recall of 0.88, and F1-score of 0.86 with 94 instances. Overall, the model achieved an accuracy of approximately 88% across 226 test samples, indicating strong performance in classifying concrete strength levels.

![image](https://github.com/user-attachments/assets/f0ebfd43-b92b-4e75-889d-b7bab41c5da0)

Confusion Matrix: A confusion matrix is created to visualize the model's predictions versus the actual labels. The confusion matrix reveals the following:

True positives for "high" are 69, "low" are 46, and "medium" are 83.
False positives are relatively low, with the highest being 14 instances where "medium" was incorrectly predicted as "high." This matrix confirms that the model correctly identifies most classes while showing manageable misclassifications.

![image](https://github.com/user-attachments/assets/60b30aff-cd68-45a6-ba93-e09dcd6e3727)

Training and Validation Loss Plot: The training and validation loss are plotted to assess overfitting or underfitting. The resulting graph illustrates that both losses overlap significantly and that the validation loss consistently decreases over epochs. This suggests that the model is fitting well to the data without indications of overfitting or underfitting.

![image](https://github.com/user-attachments/assets/26d66a78-1836-4fcc-8daf-b0b8ecbdcb87)

The code implements a comprehensive neural network model to classify concrete strength levels into three categories: low, medium, and high. After preprocessing the data, including normalizing features and encoding labels, the model is trained and evaluated. The model achieved an accuracy of 88% on the test dataset, demonstrating effective classification capabilities. The classification report indicates strong precision, recall, and F1-scores across all classes, with a confusion matrix providing further insights into prediction performance. Additionally, the training and validation loss analysis shows that the model maintains a healthy fit without overfitting or underfitting. Overall, the results affirm the robustness of the model in classifying concrete strength based on the provided features.
