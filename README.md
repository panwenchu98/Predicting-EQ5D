# predicting-EQ5D
Functions for predicting EQ-5D score from DASH or MHQ score
Be sure to download the code and the RData files and put them in same folder before using




The code contains:
Function for predicting EQ-5D score using DASH or MHQ:
predict_EQ5D(data, score, method, filename)

Input:
data		a data frame containing the data used for prediction
		Each row represents a patient. Should have columns Age, Gender, Diagnosis, and MHQ_Injured (or DASH).
		For the Diagnosis column, entries should lie in 1 (Carpal Tunnel Syndrome), 2 (Thumb Arthritis), 3 (Distal Radius Fracture) ,4 (Hand Tendon Injury) ,5 (Trigger Finger).
score		a string indicating which score is used for prediction. Valid values are 'MHQ' and 'DASH'
method		a string indicating the prediction methods used.
		Valid values are 'OLS', 'Tobit', 'TPM', 'TPM-L', 'RM-L' and 'RM-NB'.
		Default methods for MHQ is RM-L, for DASH is OLS.
filename	a string indicating the model file used for prediction.
		Default is NULL and the code will use the models fitted from the data used in the paper.
Output:
a data frame which consists of the input data and a column 'predict' which contains the predicted EQ-5D score.





Function for fitting prediction models using own data:
fit_EQ5D(data, score, methods, filename)

Input:
data		a data frame containing the data used for model fitting.
		Each row represents a patient. Should have columns Age, Gender, Diagnosis, and MHQ_Injured (or DASH).
		For the Diagnosis column, entries should lie in 1 (Carpal Tunnel Syndrome), 2 (Thumb Arthritis), 3 (Distal Radius Fracture) ,4 (Hand Tendon Injury) ,5 (Trigger Finger).
		If wanting to fit for Response Modeling, should have columns Mobility, SelfCare, UsusalActivities, PainDiscomfort, AnxietyDepression. The entries of these columns should be in 1,2,3,4,5.
score		a string indicating which score is used for model fitting. Valid values are 'MHQ' and 'DASH'
methods		a length 6 array of TRUE/FALSE, indicating which models the user wants to fit.
		Each entry corresponds to OLS, Tobit, TPM, TPM-L, RM-L and RM-NB respectively.
		The default is (TRUE, TRUE, TRUE, TRUE, FALSE, FALSE), which indicates using OLS, Tobit, TPM, TPM-L.
filename	a string indicating the file name to save the fitted model. 
		Default is 'CustomModel.RData'.
Output:
This function has no output and an RData file should be generated which can be used in the predict_EQ5D function.






Example:
The below code fits all 6 models using data_train that predicts EQ-5D from MHQ score of injured hand, and save the models to "new_models.RData". Then load the fitted model and makes prediction for data_test using the TPM-L model.

fit_EQ5D(data = data_train, score = "MHQ", methods = rep(TRUE,6), filename = "new_models.RData")
data_test_new = predict_EQ5D(data = data_test, score = "MHQ", method = "TPM-L", filename = "new_models.RData")
