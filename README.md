# Project1
Here in this project we are using EHR-pancreatic data. In this dataset The column1 - "Sensitization type" is the dependent variable (Widespread and Segmental). 
The dataset is imbalanced.
We are developing a simple classification algorithms using a simple 3-layer Neural network, two-layer CNN, and a two-layer LSTM respectively.
Also applied techniques to get better output, like (a) k-fold cross-validation, (b) leave one subject out cross-validation and (c) k-stratified cross-validation.
Also computed Compute Performance metrics namely F-measure, precision, recall and ROC curve with a confusion matrix.
1. Develop simple classification algorithms using a simple 3-layer Neural network, two-layer CNN, and a
two-layer LSTM on the following databases: 


#importing libraries
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import MinMaxScaler
from keras.models import Sequential
from keras.layers import Dense
import numpy as np
from sklearn.impute import SimpleImputer
# Load the dataset
df = pd.read_csv('EHR_Data.csv')
#print(df)

#Cleaning and Encoding the Data
# Binary encode the 'Sensitization type' column
df['Sensitization type'] = df['Sensitization type'].map({'Widespread sensitiza
tion': 1, 'Segmental sensitization': 0})
# Print the updated dataframe
print(df)
# replace missing values in 'Gender' column with 'Unknown'
df['Gender'].fillna('Unknown', inplace=True)
# remove leading/trailing white space and convert to lowercase
df['Gender'] = df['Gender'].str.strip().str.lower()
# one-hot encode the 'Gender' column
onehot = pd.get_dummies(df['Gender'], prefix='Gender')
# concatenate the one-hot encoded 'Gender' columns with the original dataset
df = pd.concat([df, onehot], axis=1)
# drop the original 'Gender' column
df.drop('Gender', axis=1, inplace=True)
# print the updated dataset
#print(df)

# replace missing values in 'Etiology' column with 'Unknown'
df['Etiology'].fillna('Unknown', inplace=True)
# remove leading/trailing white space and convert to lowercase
df['Etiology'] = df['Etiology'].str.strip().str.lower()

# one-hot encode the 'Etiology' column
onehot = pd.get_dummies(df['Etiology'], prefix='Etiology')
# concatenate the one-hot encoded 'Etiology' columns with the original dataset
df = pd.concat([df, onehot], axis=1)
# drop the original 'Etiology' column
df.drop('Etiology', axis=1, inplace=True)
# print the updated dataset
#print(df)

# replace missing values in 'Painless/painfull' column with 'Unknown'
df['Painless/painfull'].fillna('Unknown', inplace=True)
# remove leading/trailing white space and convert to lowercase
df['Painless/painfull'] = df['Painless/painfull'].str.strip().str.lower()
# one-hot encode the 'Painless/painfull' column
onehot = pd.get_dummies(df['Painless/painfull'], prefix='Painless/painfull')

# concatenate the one-
hot encoded 'Painless/painfull' columns with the original dataset

df = pd.concat([df, onehot], axis=1)
# drop the original 'Painless/painfull' column
df.drop('Painless/painfull', axis=1, inplace=True)
# print the updated dataset
#print(df)
# create an instance of SimpleImputer with mean strategy
imputer = SimpleImputer(strategy='mean')
# fill in the missing values in the column with mean value
df['Duration betwn sx and Dx (months)'] = imputer.fit_transform(df[['Duration
betwn sx and Dx (months)']])
# print the updated dataset
#print(df)

# replace missing values in 'Diabetes' column with 'Unknown'
df['Diabetes'].fillna('Unknown', inplace=True)
# remove leading/trailing white space and convert to lowercase
df['Diabetes'] = df['Diabetes'].str.strip().str.lower()
# one-hot encode the 'Diabetes' column
onehot = pd.get_dummies(df['Diabetes'], prefix='Diabetes')
# concatenate the one-hot encoded 'Diabetes' columns with the original dataset
df = pd.concat([df, onehot], axis=1)

# drop the original 'Diabetes' column
df.drop('Diabetes', axis=1, inplace=True)
# print the updated dataset
#print(df)
# create an instance of SimpleImputer with mean strategy
imputer = SimpleImputer(strategy='mean')
# fill in the missing values in the column with mean value
df['Duration betwn CP Dx and diabetes Dx (months)'] = imputer.fit_transform(df
[['Duration betwn CP Dx and diabetes Dx (months)']])
# print the updated dataset
#print(df)

# replace missing values in 'Overall_Interventions_yes_no' column with 'Unknow
n'
df['Overall_Interventions_yes_no'].fillna('Unknown', inplace=True)
# remove leading/trailing white space and convert to lowercase
df['Overall_Interventions_yes_no'] = df['Overall_Interventions_yes_no'].str.st
rip().str.lower()
# one-hot encode the 'Overall_Interventions_yes_no' column
onehot = pd.get_dummies(df['Overall_Interventions_yes_no'], prefix='Overall_In
terventions_yes_no')

# concatenate the one-
hot encoded 'Overall_Interventions_yes_no' columns with the original dataset

df = pd.concat([df, onehot], axis=1)
# drop the original 'Overall_Interventions_yes_no' column
df.drop('Overall_Interventions_yes_no', axis=1, inplace=True)
# print the updated dataset
#print(df)

# replace missing values in 'Pancreatic_Atrophy' column with 'Unknown'
df['Pancreatic_Atrophy'].fillna('Unknown', inplace=True)
# remove leading/trailing white space and convert to lowercase
df['Pancreatic_Atrophy'] = df['Pancreatic_Atrophy'].str.strip().str.lower()
# one-hot encode the 'Pancreatic_Atrophy' column
onehot = pd.get_dummies(df['Pancreatic_Atrophy'], prefix='Pancreatic_Atrophy')

# concatenate the one-
hot encoded 'Pancreatic_Atrophy' columns with the original dataset

df = pd.concat([df, onehot], axis=1)

# drop the original 'Pancreatic_Atrophy' column
df.drop('Pancreatic_Atrophy', axis=1, inplace=True)
# replace missing values in 'Dilatation_of_MPD' column with 'Unknown'
df['Dilatation_of_MPD'].fillna('Unknown', inplace=True)
# remove leading/trailing white space and convert to lowercase
df['Dilatation_of_MPD'] = df['Dilatation_of_MPD'].str.strip().str.lower()
# one-hot encode the 'Dilatation_of_MPD' column
onehot = pd.get_dummies(df['Dilatation_of_MPD'], prefix='Dilatation_of_MPD')

# concatenate the one-
hot encoded 'Dilatation_of_MPD' columns with the original dataset

df = pd.concat([df, onehot], axis=1)
# drop the original 'Dilatation_of_MPD' column
df.drop('Dilatation_of_MPD', axis=1, inplace=True)

# replace missing values in 'MPD_stricture' column with 'Unknown'
df['MPD_stricture'].fillna('Unknown', inplace=True)
# remove leading/trailing white space and convert to lowercase
df['MPD_stricture'] = df['MPD_stricture'].str.strip().str.lower()
# one-hot encode the 'MPD_stricture' column
onehot = pd.get_dummies(df['MPD_stricture'], prefix='MPD_stricture')

# concatenate the one-
hot encoded 'MPD_stricture' columns with the original dataset

df = pd.concat([df, onehot], axis=1)
# drop the original 'MPD_stricture' column
df.drop('MPD_stricture', axis=1, inplace=True)
# replace missing values in 'MPD_calculus' column with 'Unknown'
df['MPD_calculus'].fillna('Unknown', inplace=True)
# remove leading/trailing white space and convert to lowercase
df['MPD_calculus'] = df['MPD_calculus'].str.strip().str.lower()
# one-hot encode the 'MPD_calculus' column
onehot = pd.get_dummies(df['MPD_calculus'], prefix='MPD_calculus')

# concatenate the one-
hot encoded 'MPD_calculus' columns with the original dataset

df = pd.concat([df, onehot], axis=1)
# drop the original 'MPD_calculus' column
df.drop('MPD_calculus', axis=1, inplace=True)
# replace missing values in 'Parenchymal_calcification_' column with 'Unknown'

df['Parenchymal_calcification_'].fillna('Unknown', inplace=True)
# remove leading/trailing white space and convert to lowercase
df['Parenchymal_calcification_'] = df['Parenchymal_calcification_'].str.strip(
).str.lower()
# one-hot encode the 'Parenchymal_calcification_' column
onehot = pd.get_dummies(df['Parenchymal_calcification_'], prefix='Parenchymal_
calcification_')

# concatenate the one-
hot encoded 'Parenchymal_calcification_' columns with the original dataset

df = pd.concat([df, onehot], axis=1)
# drop the original 'Parenchymal_calcification_' column
df.drop('Parenchymal_calcification_', axis=1, inplace=True)
# replace missing values in 'Radiation_of_pain' column with 'Unknown'
df['Radiation_of_pain'].fillna('Unknown', inplace=True)
# remove leading/trailing white space and convert to lowercase
df['Radiation_of_pain'] = df['Radiation_of_pain'].str.strip().str.lower()
# one-hot encode the 'Radiation_of_pain' column
onehot = pd.get_dummies(df['Radiation_of_pain'], prefix='Radiation_of_pain')

# concatenate the one-
hot encoded 'Radiation_of_pain' columns with the original dataset

df = pd.concat([df, onehot], axis=1)
# drop the original 'Radiation_of_pain' column
df.drop('Radiation_of_pain', axis=1, inplace=True)
# replace missing values in 'Development_of_new_areas_of_pain' column with 'Un
known'
df['Development_of_new_areas_of_pain'].fillna('Unknown', inplace=True)
# remove leading/trailing white space and convert to lowercase
df['Development_of_new_areas_of_pain'] = df['Development_of_new_areas_of_pain'
].str.strip().str.lower()
# one-hot encode the 'Development_of_new_areas_of_pain' column
onehot = pd.get_dummies(df['Development_of_new_areas_of_pain'], prefix='Develo
pment_of_new_areas_of_pain')

# concatenate the one-
hot encoded 'Development_of_new_areas_of_pain' columns with the original datas

et
df = pd.concat([df, onehot], axis=1)
# drop the original 'Development_of_new_areas_of_pain' column
df.drop('Development_of_new_areas_of_pain', axis=1, inplace=True)

# replace missing values in 'Overall_Pain_Pattern' column with 'Unknown'
df['Overall_Pain_Pattern'].fillna('Unknown', inplace=True)
# remove leading/trailing white space and convert to lowercase
df['Overall_Pain_Pattern'] = df['Overall_Pain_Pattern'].str.strip().str.lower(
)
# one-hot encode the 'Overall_Pain_Pattern' column
onehot = pd.get_dummies(df['Overall_Pain_Pattern'], prefix='Overall_Pain_Patte
rn')

# concatenate the one-
hot encoded 'Overall_Pain_Pattern' columns with the original dataset

df = pd.concat([df, onehot], axis=1)
# drop the original 'Overall_Pain_Pattern' column
df.drop('Overall_Pain_Pattern', axis=1, inplace=True)

# replace missing values in 'Pain continuous intermitent' column with 'Unknown
'
df['Pain continuous intermitent'].fillna('Unknown', inplace=True)
# remove leading/trailing white space and convert to lowercase
df['Pain continuous intermitent'] = df['Pain continuous intermitent'].str.stri
p().str.lower()
# one-hot encode the 'Pain continuous intermitent' column
onehot = pd.get_dummies(df['Pain continuous intermitent'], prefix='Pain contin
uous intermitent')

# concatenate the one-
hot encoded 'Pain continuous intermitent' columns with the original dataset

df = pd.concat([df, onehot], axis=1)
# drop the original 'Pain continuous intermitent' column
df.drop('Pain continuous intermitent', axis=1, inplace=True)

# print the updated dataset
#print(df)

# create an instance of SimpleImputer with mean strategy
imputer = SimpleImputer(strategy='mean')
# fill in the missing values in the column with mean value
df['Rumination'] = imputer.fit_transform(df[['Rumination']])
# create an instance of SimpleImputer with mean strategy
imputer = SimpleImputer(strategy='mean')
# fill in the missing values in the column with mean value
df['Magnification'] = imputer.fit_transform(df[['Magnification']])

# create an instance of SimpleImputer with mean strategy
imputer = SimpleImputer(strategy='mean')
# fill in the missing values in the column with mean value
df['Helplessness'] = imputer.fit_transform(df[['Helplessness']])
# create an instance of SimpleImputer with mean strategy
imputer = SimpleImputer(strategy='mean')
# fill in the missing values in the column with mean value
df['IZBICKI_Final_Score'] = imputer.fit_transform(df[['IZBICKI_Final_Score']])
# create an instance of SimpleImputer with mean strategy
imputer = SimpleImputer(strategy='mean')
# fill in the missing values in the column with mean value
df['HADS_Depression_Score_'] = imputer.fit_transform(df[['HADS_Depression_Scor
e_']])
# replace missing values in 'HADS_depression_category' column with 'Unknown'
df['HADS_depression_category'].fillna('Unknown', inplace=True)
# remove leading/trailing white space and convert to lowercase
df['HADS_depression_category'] = df['HADS_depression_category'].str.strip().st
r.lower()
# one-hot encode the 'HADS_depression_category' column
onehot = pd.get_dummies(df['HADS_depression_category'], prefix='HADS_depressio
n_category')

# concatenate the one-
hot encoded 'HADS_depression_category' columns with the original dataset

df = pd.concat([df, onehot], axis=1)
# drop the original 'HADS_depression_category' column
df.drop('HADS_depression_category', axis=1, inplace=True)
# create an instance of SimpleImputer with mean strategy
imputer = SimpleImputer(strategy='mean')
# fill in the missing values in the column with mean value
df['HADS_Anxiety_score'] = imputer.fit_transform(df[['HADS_Anxiety_score']])

# replace missing values in 'HADS_anxiety_category' column with 'Unknown'
df['HADS_anxiety_category'].fillna('Unknown', inplace=True)
# remove leading/trailing white space and convert to lowercase
df['HADS_anxiety_category'] = df['HADS_anxiety_category'].str.strip().str.lowe
r()
# one-hot encode the 'HADS_anxiety_category' column

onehot = pd.get_dummies(df['HADS_anxiety_category'], prefix='HADS_anxiety_cate
gory')

# concatenate the one-
hot encoded 'HADS_anxiety_category' columns with the original dataset

df = pd.concat([df, onehot], axis=1)
# drop the original 'HADS_anxiety_category' column
df.drop('HADS_anxiety_category', axis=1, inplace=True)

# create an instance of SimpleImputer with mean strategy
imputer = SimpleImputer(strategy='mean')
# fill in the missing values in the column with mean value
df['Pain_DETECT_final_score'] = imputer.fit_transform(df[['Pain_DETECT_final_s
core']])

# replace missing values in 'PainDETECT_typeofpain' column with 'Unknown'
df['PainDETECT_typeofpain'].fillna('Unknown', inplace=True)
# remove leading/trailing white space and convert to lowercase
df['PainDETECT_typeofpain'] = df['PainDETECT_typeofpain'].str.strip().str.lowe
r()
# one-hot encode the 'PainDETECT_typeofpain' column
onehot = pd.get_dummies(df['PainDETECT_typeofpain'], prefix='PainDETECT_typeof
pain')

# concatenate the one-
hot encoded 'PainDETECT_typeofpain' columns with the original dataset

df = pd.concat([df, onehot], axis=1)
# drop the original 'PainDETECT_typeofpain' column
df.drop('PainDETECT_typeofpain', axis=1, inplace=True)

# create an instance of SimpleImputer with mean strategy
imputer = SimpleImputer(strategy='mean')
# fill in the missing values in the column with mean value
df['BDI_II_score'] = imputer.fit_transform(df[['BDI_II_score']])
# replace missing values in 'BDI_II_Category' column with 'Unknown'
df['BDI_II_Category'].fillna('Unknown', inplace=True)
# remove leading/trailing white space and convert to lowercase
df['BDI_II_Category'] = df['BDI_II_Category'].str.strip().str.lower()
# one-hot encode the 'BDI_II_Category' column
onehot = pd.get_dummies(df['BDI_II_Category'], prefix='BDI_II_Category')

# concatenate the one-
hot encoded 'BDI_II_Category' columns with the original dataset

df = pd.concat([df, onehot], axis=1)
# drop the original 'BDI_II_Category' column
df.drop('BDI_II_Category', axis=1, inplace=True)
# create an instance of SimpleImputer with mean strategy
imputer = SimpleImputer(strategy='mean')
# fill in the missing values in the column with mean value
df['EORTC_Global_Score'] = imputer.fit_transform(df[['EORTC_Global_Score']])
# fill in the missing values in the column with mean value
df['EORTC_Fatigue'] = imputer.fit_transform(df[['EORTC_Fatigue']])
# fill in the missing values in the column with mean value
df['EORTC_Nausea_Vomiting'] = imputer.fit_transform(df[['EORTC_Nausea_Vomiting
']])
# fill in the missing values in the column with mean value
df['EORTC_Pain'] = imputer.fit_transform(df[['EORTC_Pain']])
# fill in the missing values in the column with mean value
df['EORTC_Dyspnoea'] = imputer.fit_transform(df[['EORTC_Dyspnoea']])
# fill in the missing values in the column with mean value
df['EORTC_Insomnia'] = imputer.fit_transform(df[['EORTC_Insomnia']])
# fill in the missing values in the column with mean value
df['EORTC_Appetite_loss'] = imputer.fit_transform(df[['EORTC_Appetite_loss']])
# fill in the missing values in the column with mean value
df['EORTC_Constipation'] = imputer.fit_transform(df[['EORTC_Constipation']])
# fill in the missing values in the column with mean value
df['EORTC_Diarrhoea'] = imputer.fit_transform(df[['EORTC_Diarrhoea']])
# fill in the missing values in the column with mean value
df['EORTC_Financial_difficulties'] = imputer.fit_transform(df[['EORTC_Financia
l_difficulties']])
# fill in the missing values in the column with mean value
df['EORTC_Physical'] = imputer.fit_transform(df[['EORTC_Physical']])
# fill in the missing values in the column with mean value
df['EORTC_Role'] = imputer.fit_transform(df[['EORTC_Role']])
# fill in the missing values in the column with mean value
df['EORTC_Emotional'] = imputer.fit_transform(df[['EORTC_Emotional']])
# fill in the missing values in the column with mean value
df['EORTC_Cognitive'] = imputer.fit_transform(df[['EORTC_Cognitive']])
# fill in the missing values in the column with mean value
df['EORTC_Social'] = imputer.fit_transform(df[['EORTC_Social']])

# fill in the missing values in the column with mean value
df['PAN28_pancreatic_pain'] = imputer.fit_transform(df[['PAN28_pancreatic_pain
']])
# fill in the missing values in the column with mean value
df['PAN28_digestive_sym'] = imputer.fit_transform(df[['PAN28_digestive_sym']])
# fill in the missing values in the column with mean value
df['PAN28_altered_bowel'] = imputer.fit_transform(df[['PAN28_altered_bowel']])
# fill in the missing values in the column with mean value
df['PAN28_Hepatic'] = imputer.fit_transform(df[['PAN28_Hepatic']])
# fill in the missing values in the column with mean value
df['PAN28_Bodyimage'] = imputer.fit_transform(df[['PAN28_Bodyimage']])
# fill in the missing values in the column with mean value
df['PAN28_Activity'] = imputer.fit_transform(df[['PAN28_Activity']])
# fill in the missing values in the column with mean value
df['PAN28_Satisfaction_with_healthcare'] = imputer.fit_transform(df[['PAN28_Sa
tisfaction_with_healthcare']])
# fill in the missing values in the column with mean value
df['PAN28_Sexuality'] = imputer.fit_transform(df[['PAN28_Sexuality']])

# replace missing values in 'P-QST_Yes_No' column with 'Unknown'
df['P-QST_Yes_No'].fillna('Unknown', inplace=True)
# remove leading/trailing white space and convert to lowercase
df['P-QST_Yes_No'] = df['P-QST_Yes_No'].str.strip().str.lower()
# one-hot encode the 'P-QST_Yes_No' column
onehot = pd.get_dummies(df['P-QST_Yes_No'], prefix='P-QST_Yes_No')

# concatenate the one-hot encoded 'P-
QST_Yes_No' columns with the original dataset

df = pd.concat([df, onehot], axis=1)
# drop the original 'P-QST_Yes_No' column
df.drop('P-QST_Yes_No', axis=1, inplace=True)

# replace missing values in 'Sensitization_yes_no' column with 'Unknown'
df['Sensitization_yes_no'].fillna('Unknown', inplace=True)
# remove leading/trailing white space and convert to lowercase
df['Sensitization_yes_no'] = df['Sensitization_yes_no'].str.strip().str.lower(
)
# one-hot encode the 'Sensitization_yes_no' column
onehot = pd.get_dummies(df['Sensitization_yes_no'], prefix='Sensitization_yes_
no')

# concatenate the one-
hot encoded 'Sensitization_yes_no' columns with the original dataset

df = pd.concat([df, onehot], axis=1)
# drop the original 'Sensitization_yes_no' column
df.drop('Sensitization_yes_no', axis=1, inplace=True)
# print the updated dataset
print(df)


3-layer Neural network-

import pandas as pd

import numpy as np
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split
from keras.models import Sequential
from keras.layers import Dense
from keras.callbacks import EarlyStopping
from sklearn.utils import resample
# Load the dataset
data = df
# Split the dataset into X (independent variables) and y (dependent variable)
X = data.iloc[:, 1:].values
y = data.iloc[:, 0].values
# Split the dataset into training and testing sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, rando
m_state=42)
# Define the model architecture
model = Sequential()
model.add(Dense(units=64, activation='relu', input_dim=X_train.shape[1]))
model.add(Dense(units=32, activation='relu'))
model.add(Dense(units=1, activation='sigmoid'))
# Compile the model
model.compile(loss='binary_crossentropy', optimizer='adam', metrics=['accuracy
'])
# Set up early stopping
early_stopping = EarlyStopping(monitor='val_loss', patience=5)

# Train the model
history = model.fit(X_train, y_train, epochs=100, batch_size=32, validation_da
ta=(X_test, y_test), callbacks=[early_stopping])
# Evaluate the model on the test set
loss, accuracy = model.evaluate(X_test, y_test)
print('Test loss:', loss)
print('Test accuracy:', accuracy)
# Plot accuracy and loss curves
plt.plot(history.history['accuracy'])
plt.plot(history.history['val_accuracy'])
plt.title('Model Accuracy')

plt.ylabel('Accuracy')
plt.xlabel('Epoch')
plt.legend(['Train', 'Validation'], loc='upper left')
plt.show()
plt.plot(history.history['loss'])
plt.plot(history.history['val_loss'])
plt.title('Model Loss')
plt.ylabel('Loss')
plt.xlabel('Epoch')
plt.legend(['Train', 'Validation'], loc='upper left')
plt.show()
# Make predictions on the test set
y_pred = model.predict(X_test)
y_pred = (y_pred > 0.5)
# Compute performance metrics
from sklearn.metrics import confusion_matrix, precision_score, recall_score, f
1_score, roc_curve, roc_auc_score
cm = confusion_matrix(y_test, y_pred)
precision = precision_score(y_test, y_pred)
recall = recall_score(y_test, y_pred)
f1 = f1_score(y_test, y_pred)
fpr, tpr, thresholds = roc_curve(y_test, y_pred)
roc_auc = roc_auc_score(y_test, y_pred)
print('Confusion matrix:\n', cm)
print('Precision:', precision)
print('Recall:', recall)
print('F1-score:', f1)
print('ROC AUC:', roc_auc)
plt.plot(fpr, tpr, 'b', label='ROC curve (area = %0.2f)' % roc_auc)
plt.plot([0, 1], [0, 1], 'r--')
plt.xlim([0, 1])
plt.ylim([0, 1])
plt.xlabel('False Positive Rate')
plt.ylabel('True Positive Rate')
plt.title('Receiver operating characteristic')
plt.legend(loc='lower right')
plt.show()



Two-layer CNN -

#CNN
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split
from keras.models import Sequential
from keras.layers import Dense, Conv1D, Flatten, MaxPooling1D
from keras.callbacks import EarlyStopping
from sklearn.utils import resample
# Load the dataset
data = df
# Split the dataset into X (independent variables) and y (dependent variable)
X = data.iloc[:, 1:].values
y = data.iloc[:, 0].values
# Split the dataset into training and testing sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, rando
m_state=42)
#Reshape input data for CNN
X_train = X_train.reshape(X_train.shape[0], X_train.shape[1], 1)
X_test = X_test.reshape(X_test.shape[0], X_test.shape[1], 1)

# Define the model architecture
model = Sequential()
model.add(Conv1D(filters=64, kernel_size=3, activation='relu', input_shape=X_t
rain.shape[1:]))
model.add(MaxPooling1D(pool_size=2))
model.add(Conv1D(filters=32, kernel_size=3, activation='relu'))
model.add(MaxPooling1D(pool_size=2))
model.add(Flatten())
model.add(Dense(units=1, activation='sigmoid'))
# Compile the model
model.compile(loss='binary_crossentropy', optimizer='adam', metrics=['accuracy
'])
# Set up early stopping
early_stopping = EarlyStopping(monitor='val_loss', patience=5)

# Train the model
history = model.fit(X_train, y_train, epochs=100, batch_size=32, validation_da
ta=(X_test, y_test), callbacks=[early_stopping])
# Evaluate the model on the test set
loss, accuracy = model.evaluate(X_test, y_test)
print('Test loss:', loss)
print('Test accuracy:', accuracy)
# Plot accuracy and loss curves
plt.plot(history.history['accuracy'])
plt.plot(history.history['val_accuracy'])
plt.title('Model Accuracy')
plt.ylabel('Accuracy')
plt.xlabel('Epoch')
plt.legend(['Train', 'Validation'], loc='upper left')
plt.show()
plt.plot(history.history['loss'])
plt.plot(history.history['val_loss'])
plt.title('Model Loss')
plt.ylabel('Loss')
plt.xlabel('Epoch')
plt.legend(['Train', 'Validation'], loc='upper left')
plt.show()
# Make predictions on the test set
y_pred = model.predict(X_test)
y_pred = (y_pred > 0.5)
# Compute performance metrics
from sklearn.metrics import confusion_matrix, precision_score, recall_score, f
1_score, roc_curve, roc_auc_score
cm = confusion_matrix(y_test, y_pred)
precision = precision_score(y_test, y_pred)

recall = recall_score(y_test, y_pred)
f1 = f1_score(y_test, y_pred)
fpr, tpr, thresholds = roc_curve(y_test, y_pred)
roc_auc = roc_auc_score(y_test, y_pred)
print('Confusion matrix:\n', cm)
print('Precision:', precision)
print('Recall:', recall)
print('F1-score:', f1)
print('ROC AUC:', roc_auc)
plt.plot(fpr, tpr, 'b', label='ROC curve (area = %0.2f)' % roc_auc)
plt.plot([0, 1], [0, 1], 'r--')
plt.xlim([0, 1])
plt.ylim([0, 1])
plt.xlabel('False Positive Rate')
plt.ylabel('True Positive Rate')
plt.title('Receiver operating characteristic')
plt.legend(loc='lower right')
plt.show()



A two-layer LSTM -
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split
from keras.models import Sequential
from keras.layers import Dense, LSTM
from keras.callbacks import EarlyStopping
from sklearn.utils import resample
# Load the dataset
data = df
# Split the dataset into X (independent variables) and y (dependent variable)
X = data.iloc[:, 1:].values
y = data.iloc[:, 0].values
# Split the dataset into training and testing sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, rando
m_state=42)
# Resample the training data to address class imbalance
X_train_resampled, y_train_resampled = resample(X_train[y_train==0], y_train[y
_train==0],
replace=True, n_samples=X_trai
n[y_train==1].shape[0],
random_state=42)
X_train_resampled = np.concatenate([X_train_resampled, X_train[y_train==1]], a
xis=0)
y_train_resampled = np.concatenate([y_train_resampled, y_train[y_train==1]], a
xis=0)
# Define the model architecture
model = Sequential()
model.add(LSTM(units=64, return_sequences=True, input_shape=(X_train.shape[1],
1)))
model.add(LSTM(units=32))
model.add(Dense(units=1, activation='sigmoid'))
# Compile the model
model.compile(loss='binary_crossentropy', optimizer='adam', metrics=['accuracy
'])
# Set up early stopping
early_stopping = EarlyStopping(monitor='val_loss', patience=5)
# Train the model
history = model.fit(X_train_resampled.reshape(-
1, X_train.shape[1], 1), y_train_resampled, epochs=100, batch_size=32, validat
ion_data=(X_test.reshape(-
1, X_train.shape[1], 1), y_test), callbacks=[early_stopping])

# Evaluate the model on the test set
loss, accuracy = model.evaluate(X_test.reshape(-
1, X_train.shape[1], 1), y_test)
print('Test loss:', loss)
print('Test accuracy:', accuracy)
# Plot accuracy and loss curves
plt.plot(history.history['accuracy'])
plt.plot(history.history['val_accuracy'])
plt.title('Model Accuracy')
plt.ylabel('Accuracy')
plt.xlabel('Epoch')
plt.legend(['Train', 'Validation'], loc='upper left')
plt.show()
plt.plot(history.history['loss'])
plt.plot(history.history['val_loss'])
plt.title('Model Loss')
plt.ylabel('Loss')
plt.xlabel('Epoch')
plt.legend(['Train', 'Validation'], loc='upper left')
plt.show()
# Make predictions on the test set
y_pred = model.predict(X_test.reshape(-1, X_train.shape[1], 1))
y_pred = (y_pred > 0.5)
# Compute performance metrics
from sklearn.metrics import confusion_matrix, precision_score, recall_score, f
1_score, roc_curve, roc_auc_score
cm = confusion_matrix(y_test, y_pred)
precision = precision_score(y_test, y_pred)
recall = recall_score(y_test, y_pred)
f1 = f1_score(y_test, y_pred)
fpr, tpr, thresholds = roc_curve(y_test, y_pred)
roc_auc = roc_auc_score(y_test, y_pred)
print('Confusion matrix:\n', cm)
print('Precision:', precision)
print('Recall:', recall)
print('F1-score:', f1)
print('ROC AUC:', roc_auc)
plt.plot(fpr, tpr, 'b', label='ROC curve (area = %0.2f)' % roc_auc)
plt.plot([0, 1], [0, 1], 'r--')
plt.xlim([0, 1])
plt.ylim([0, 1])
plt.xlabel('False Positive Rate')
plt.ylabel('True Positive Rate')
plt.title('Receiver operating characteristic')
plt.legend(loc='lower right')
plt.show()



2. Data is imbalanced. Use the strategy to overcome the challenge of imbalance
(a) undersampling, (b) oversampling

(a) Undersampling applied to simple 3-layer Neural network-
import pandas as pd

import numpy as np
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split
from keras.models import Sequential
from keras.layers import Dense
from keras.callbacks import EarlyStopping
from sklearn.utils import resample
# Load the dataset
data = df
# Split the dataset into X (independent variables) and y (dependent variable)
X = data.iloc[:, 1:].values
y = data.iloc[:, 0].values

# Undersample the majority class
from imblearn.under_sampling import RandomUnderSampler
rus = RandomUnderSampler(random_state=42)
X_resampled, y_resampled = rus.fit_resample(X, y)

# Split the dataset into training and testing sets
X_train, X_test, y_train, y_test = train_test_split(X_resampled, y_resampled,
test_size=0.2, random_state=42)

# Define the model architecture
model = Sequential()
model.add(Dense(units=64, activation='relu', input_dim=X_train.shape[1]))
model.add(Dense(units=32, activation='relu'))
model.add(Dense(units=1, activation='sigmoid'))
# Compile the model
model.compile(loss='binary_crossentropy', optimizer='adam', metrics=['accuracy
'])
# Set up early stopping
early_stopping = EarlyStopping(monitor='val_loss', patience=5)

# Train the model
history = model.fit(X_train, y_train, epochs=100, batch_size=32, validation_da
ta=(X_test, y_test), callbacks=[early_stopping])
# Evaluate the model on the test set
loss, accuracy = model.evaluate(X_test, y_test)
print('Test loss:', loss)
print('Test accuracy:', accuracy)
# Plot accuracy and loss curves
plt.plot(history.history['accuracy'])
plt.plot(history.history['val_accuracy'])
plt.title('Model Accuracy')
plt.ylabel('Accuracy')
plt.xlabel('Epoch')
plt.legend(['Train', 'Validation'], loc='upper left')
plt.show()
plt.plot(history.history['loss'])
plt.plot(history.history['val_loss'])
plt.title('Model Loss')
plt.ylabel('Loss')
plt.xlabel('Epoch')
plt.legend(['Train', 'Validation'], loc='upper left')
plt.show()
# Make predictions on the test set
y_pred = model.predict(X_test)

y_pred = (y_pred > 0.5)
# Compute performance metrics
from sklearn.metrics import confusion_matrix, precision_score, recall_score, f
1_score, roc_curve, roc_auc_score
cm = confusion_matrix(y_test, y_pred)
precision = precision_score(y_test, y_pred)
recall = recall_score(y_test, y_pred)
f1 = f1_score(y_test, y_pred)
fpr, tpr, thresholds = roc_curve(y_test, y_pred)
roc_auc = roc_auc_score(y_test, y_pred)
print('Confusion matrix:\n', cm)
print('Precision:', precision)
print('Recall:', recall)
print('F1-score:', f1)
print('ROC AUC:', roc_auc)
plt.plot(fpr, tpr, 'b', label='ROC curve (area = %0.2f)' % roc_auc)
plt.plot([0, 1], [0, 1], 'r--')
plt.xlim([0, 1])
plt.ylim([0, 1])
plt.xlabel('False Positive Rate')
plt.ylabel('True Positive Rate')
plt.title('Receiver operating characteristic')
plt.legend(loc='lower right')
plt.show()



(b) Oversampling applied to simple 3-layer Neural network-
import pandas as pd

import numpy as np
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split
from keras.models import Sequential
from keras.layers import Dense
from keras.callbacks import EarlyStopping
from sklearn.utils import resample
# Load the dataset
data = df
# Split the dataset into X (independent variables) and y (dependent variable)
X = data.iloc[:, 1:].values
y = data.iloc[:, 0].values

# Oversample the minority class
from imblearn.over_sampling import RandomOverSampler
ros = RandomOverSampler(random_state=42)
X_resampled, y_resampled = ros.fit_resample(X, y)

# Split the dataset into training and testing sets

X_train, X_test, y_train, y_test = train_test_split(X_resampled, y_resampled,
test_size=0.2, random_state=42)

# Define the model architecture
model = Sequential()
model.add(Dense(units=64, activation='relu', input_dim=X_train.shape[1]))
model.add(Dense(units=32, activation='relu'))
model.add(Dense(units=1, activation='sigmoid'))
# Compile the model
model.compile(loss='binary_crossentropy', optimizer='adam', metrics=['accuracy
'])
# Set up early stopping
early_stopping = EarlyStopping(monitor='val_loss', patience=5)

# Train the model
history = model.fit(X_train, y_train, epochs=100, batch_size=32, validation_da
ta=(X_test, y_test), callbacks=[early_stopping])
# Evaluate the model on the test set
loss, accuracy = model.evaluate(X_test, y_test)
print('Test loss:', loss)
print('Test accuracy:', accuracy)
# Plot accuracy and loss curves
plt.plot(history.history['accuracy'])
plt.plot(history.history['val_accuracy'])
plt.title('Model Accuracy')
plt.ylabel('Accuracy')
plt.xlabel('Epoch')
plt.legend(['Train', 'Validation'], loc='upper left')
plt.show()
plt.plot(history.history['loss'])
plt.plot(history.history['val_loss'])
plt.title('Model Loss')
plt.ylabel('Loss')
plt.xlabel('Epoch')
plt.legend(['Train', 'Validation'], loc='upper left')
plt.show()
# Make predictions on the test set
y_pred = model.predict(X_test)
y_pred = (y_pred > 0.5)
# Compute performance metrics
from sklearn.metrics import confusion_matrix, precision_score, recall_score, f
1_score, roc_curve, roc_auc_score
cm = confusion_matrix(y_test, y_pred)
precision = precision_score(y_test, y_pred)
recall = recall_score(y_test, y_pred)

f1 = f1_score(y_test, y_pred)
fpr, tpr, thresholds = roc_curve(y_test, y_pred)
roc_auc = roc_auc_score(y_test, y_pred)
print('Confusion matrix:\n', cm)
print('Precision:', precision)
print('Recall:', recall)
print('F1-score:', f1)
print('ROC AUC:', roc_auc)
plt.plot(fpr, tpr, 'b', label='ROC curve (area = %0.2f)' % roc_auc)
plt.plot([0, 1], [0, 1], 'r--')
plt.xlim([0, 1])
plt.ylim([0, 1])
plt.xlabel('False Positive Rate')
plt.ylabel('True Positive Rate')
plt.title('Receiver operating characteristic')
plt.legend(loc='lower right')
plt.show()



3.(a) k-fold cross-validation, (b) leave one subject out cross-validation and (c) k-stratified cross-
validation.

(a) k-fold cross-validation to simple 3-layer Neural network-
#K-Fold CV
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split, KFold
from keras.models import Sequential
from keras.layers import Dense
from keras.callbacks import EarlyStopping
from sklearn.utils import resample
# Load the dataset
data = df

# Split the dataset into X (independent variables) and y (dependent variable)
X = data.iloc[:, 1:].values
y = data.iloc[:, 0].values
# Oversample the minority class
from imblearn.over_sampling import RandomOverSampler
ros = RandomOverSampler(random_state=42)
X_resampled, y_resampled = ros.fit_resample(X, y)
# Set up k-fold cross-validation
kf = KFold(n_splits=5, shuffle=True, random_state=42)
# Define the model architecture
model = Sequential()
model.add(Dense(units=64, activation='relu', input_dim=X_resampled.shape[1]))
model.add(Dense(units=32, activation='relu'))
model.add(Dense(units=1, activation='sigmoid'))
# Compile the model
model.compile(loss='binary_crossentropy', optimizer='adam', metrics=['accuracy
'])
# Set up early stopping
early_stopping = EarlyStopping(monitor='val_loss', patience=5)
# Train and evaluate the model using k-fold cross-validation
acc_scores = []
loss_scores = []
for train_idx, test_idx in kf.split(X_resampled, y_resampled):
X_train, X_test = X_resampled[train_idx], X_resampled[test_idx]
y_train, y_test = y_resampled[train_idx], y_resampled[test_idx]

history = model.fit(X_train, y_train, epochs=100, batch_size=32, validatio
n_data=(X_test, y_test), callbacks=[early_stopping], verbose=1)
loss, accuracy = model.evaluate(X_test, y_test, verbose=0)
acc_scores.append(accuracy)
loss_scores.append(loss)
# Compute and print performance metrics
print('Accuracy:', np.mean(acc_scores))
print('Loss:', np.mean(loss_scores))
# Plot accuracy and loss curves
plt.plot(history.history['accuracy'])
plt.plot(history.history['val_accuracy'])
plt.title('Model Accuracy')
plt.ylabel('Accuracy')
plt.xlabel('Epoch')
plt.legend(['Train', 'Validation'], loc='upper left')
plt.show()
plt.plot(history.history['loss'])
plt.plot(history.history['val_loss'])
plt.title('Model Loss')
plt.ylabel('Loss')
plt.xlabel('Epoch')
plt.legend(['Train', 'Validation'], loc='upper left')
plt.show()
# Make predictions on the test set
y_pred = model.predict(X_test)
y_pred = (y_pred > 0.5)
# Compute performance metrics
from sklearn.metrics import confusion_matrix, precision_score, recall_score, f
1_score, roc_curve, roc_auc_score
cm = confusion_matrix(y_test, y_pred)
precision = precision_score(y_test, y_pred)
recall = recall_score(y_test, y_pred)
f1 = f1_score(y_test, y_pred)
fpr, tpr, thresholds = roc_curve(y_test, y_pred)
roc_auc = roc_auc_score(y_test, y_pred)
print('Confusion matrix:\n', cm)
print('Precision:', precision)
print('Recall:', recall)
print('F1-score:', f1)
print('ROC AUC:', roc_auc)
plt.plot(fpr, tpr, 'b', label='ROC curve (area = %0.2f)' % roc_auc)
plt.plot([0, 1], [0, 1], 'r--')
plt.xlim([0, 1])
plt.ylim([0, 1])
plt.xlabel('False Positive Rate')
plt.ylabel('True Positive Rate')
plt.title('Receiver operating characteristic')
plt.legend(loc='lower right')

plt.show()


(b)leave one subject out cross-validation to simple 3-layer Neural

network-
import pandas as pd

import numpy as np
import matplotlib.pyplot as plt
from sklearn.model_selection import LeaveOneGroupOut
from keras.models import Sequential
from keras.layers import Dense
from keras.callbacks import EarlyStopping
from imblearn.over_sampling import RandomOverSampler
from sklearn.utils import shuffle
# Load the dataset
data = df
# Sort the data by subject id
data = data.sort_values('Sensitization type')
# Split the dataset into X (independent variables) and y (dependent variable)
X = data.iloc[:, 1:].values
y = data.iloc[:, 0].values
groups = data.iloc[:, 1].values

# Define the model architecture
model = Sequential()
model.add(Dense(units=64, activation='relu', input_dim=X.shape[1]))
model.add(Dense(units=32, activation='relu'))
model.add(Dense(units=1, activation='sigmoid'))
# Compile the model
model.compile(loss='binary_crossentropy', optimizer='adam', metrics=['accuracy
'])
# Set up early stopping
early_stopping = EarlyStopping(monitor='val_loss', patience=5)
# Set up leave-one-subject-out cross-validation
logo = LeaveOneGroupOut()
# Train and evaluate the model with leave-one-subject-out cross-validation
for train_index, test_index in logo.split(X, y, groups=groups):
X_train, X_test = X[train_index], X[test_index]
y_train, y_test = y[train_index], y[test_index]
# Shuffle the training set
X_train, y_train = shuffle(X_train, y_train, random_state=42)
# Oversample the minority class in the training set
ros = RandomOverSampler(random_state=42)
X_train_resampled, y_train_resampled = ros.fit_resample(X_train, y_train)
# Train the model
history = model.fit(X_train_resampled, y_train_resampled, epochs=100, batc
h_size=32, validation_data=(X_test, y_test), callbacks=[early_stopping])
# Evaluate the model on the test set
loss, accuracy = model.evaluate(X_test, y_test)
print('Test loss:', loss)
print('Test accuracy:', accuracy)
# Make predictions on the test set
y_pred = model.predict(X_test)
y_pred = (y_pred > 0.5)
# Compute performance metrics
from sklearn.metrics import confusion_matrix, precision_score, recall_scor
e, f1_score, roc_curve, roc_auc_score
cm = confusion_matrix(y_test, y_pred)
precision = precision_score(y_test, y_pred)
recall = recall_score(y_test, y_pred)
f1 = f1_score(y_test, y_pred)
fpr, tpr, thresholds = roc_curve(y_test, y_pred)
roc_auc = roc_auc_score(y_test, y_pred)
print('Confusion matrix:\n', cm)
print('Precision:', precision)

print('Recall:', recall)
print('F1-score:', f1)
print('ROC AUC:', roc_auc)
plt.plot(fpr, tpr, 'b', label='ROC curve (area = %0.2f)' % roc_auc)
plt.plot([0, 1], [0, 1], 'r--')
plt.xlim([0, 1])
plt.ylim([0, 1])
plt.xlabel('False Positive Rate')
plt.ylabel('True Positive Rate')
plt.title('Receiver operating characteristic')
plt.legend(loc='lower right')
plt.show()



(c) k-stratified cross-validation to simple 3-layer Neural network-
#stratified cross-validation
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split, StratifiedKFold
from keras.models import Sequential
from keras.layers import Dense
from keras.callbacks import EarlyStopping
from sklearn.utils import resample
# Load the dataset
data = df
# Split the dataset into X (independent variables) and y (dependent variable)
X = data.iloc[:, 1:].values
y = data.iloc[:, 0].values
# Define the number of splits for stratified cross-validation
n_splits = 5
# Define the model architecture
model = Sequential()
model.add(Dense(units=64, activation='relu', input_dim=X.shape[1]))
model.add(Dense(units=32, activation='relu'))
model.add(Dense(units=1, activation='sigmoid'))
# Compile the model
model.compile(loss='binary_crossentropy', optimizer='adam', metrics=['accuracy
'])
# Set up early stopping
early_stopping = EarlyStopping(monitor='val_loss', patience=5)
# Set up stratified cross-validation
skf = StratifiedKFold(n_splits=n_splits, shuffle=True, random_state=42)
# Train and evaluate the model with stratified cross-validation
for train_index, test_index in skf.split(X, y):
X_train, X_test = X[train_index], X[test_index]
y_train, y_test = y[train_index], y[test_index]
# Oversample the minority class in the training set
ros = RandomOverSampler(random_state=42)
X_train_resampled, y_train_resampled = ros.fit_resample(X_train, y_train)
# Train the model
history = model.fit(X_train_resampled, y_train_resampled, epochs=100, batc
h_size=32, validation_data=(X_test, y_test), callbacks=[early_stopping])
# Evaluate the model on the test set
loss, accuracy = model.evaluate(X_test, y_test)
print('Test loss:', loss)

print('Test accuracy:', accuracy)
# Plot accuracy and loss curves
plt.plot(history.history['accuracy'])
plt.plot(history.history['val_accuracy'])
plt.title('Model Accuracy')
plt.ylabel('Accuracy')
plt.xlabel('Epoch')
plt.legend(['Train', 'Validation'], loc='upper left')
plt.show()
plt.plot(history.history['loss'])
plt.plot(history.history['val_loss'])
plt.title('Model Loss')
plt.ylabel('Loss')
plt.xlabel('Epoch')
plt.legend(['Train', 'Validation'], loc='upper left')
plt.show()
# Make predictions on the test set
y_pred = model.predict(X_test)
y_pred = (y_pred > 0.5)
# Compute performance metrics
from sklearn.metrics import confusion_matrix, precision_score, recall_score, f
1_score, roc_curve, roc_auc_score
cm = confusion_matrix(y_test, y_pred)
precision = precision_score(y_test, y_pred)
recall = recall_score(y_test, y_pred)
f1 = f1_score(y_test, y_pred)
fpr, tpr, thresholds = roc_curve(y_test, y_pred)
roc_auc = roc_auc_score(y_test, y_pred)
print('Confusion matrix:\n', cm)
print('Precision:', precision)
print('Recall:', recall)
print('F1-score:', f1)
print('ROC AUC:', roc_auc)
plt.plot(fpr, tpr, 'b', label='ROC curve (area = %0.2f)' % roc_auc)
plt.plot([0, 1], [0, 1], 'r--')
plt.xlim([0, 1])
plt.ylim([0, 1])
plt.xlabel('False Positive Rate')
plt.ylabel('True Positive Rate')
plt.title('Receiver operating characteristic')
plt.legend(loc='lower right')
plt.show()

