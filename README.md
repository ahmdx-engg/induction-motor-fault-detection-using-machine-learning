# FAULT DETECTION OF INDUCTION MOTOR USING MACHINE LEARNING

This project is about using machine learning in MATLAB to detect faults in an induction motor.
It helps in detecting faults in induction motors before they break down like broken rotor bars, worn bearings, or winding issues by training machine learning model on dataset to understand what's going inside motor and to take necessary steps for its predictive maintenance.

**Getting the Data Ready**
Unzip experimental recordings from induction motor into folders.
Then we use File Ensemble Datastore, its a Matlab way of handling huge collection of related datafiles. It not only stores data but keep record of which file belong to which fault condition. This makes it easy to process and compare signals.

**Signal Processing and Envelope Analysis**
Next up, we clean and process the signals.

To detect motor faults, we need to focus on the right frequency range, so we apply a band-pass filter between 900 Hz and 1300 Hz.
This range tends to capture most fault-related energy in the signal.

After that, we calculate something called an envelope. It's a smoothed version of the signal that reveals hidden frequency patterns.

We also use the Wavelet Toolbox to create beautiful scalograms, which show how the signal’s energy changes over time and frequency.
These plots make it easy to see fault patterns visually.

**Feature Extraction**
When the signals are ready, it’s time to let MATLAB’s Diagnostic Feature Designer App to:

Import the preprocessed signals.
Visualize them in both time and frequency domains.
Use the Auto Features button. MATLAB automatically extracts dozens of useful features.
Rank them using a One-way ANOVA test, which helps find which features are best at separating fault conditions.
We end up with a ranked list of features usually the top 10 are more than enough for classification.

**Training the Machine Learning Model**
Once the features are ready,export them straight into the Classification Learner App.

Inside that app:

Choose a 5-fold cross-validation setup to make sure our results are solid.
Try out multiple models i.e decision trees, SVMs, ensemble models, etc.
Click Train All, sit back, and let MATLAB test everything.

The app shows each model’s accuracy, so we just pick the one that performs best.
In our case, the KNN reached about 97.5% accuracy

**Checking Model Performance**
We use a Confusion Matrix to understand how the model did.The diagonal cells show correct predictions e.g., all healthy cases identified correctly.
The off-diagonal ones show mistakes.

Here’s what got noticed:

All healthy cases were predicted correctly.
A few minor mix-ups happened between similar fault conditions.
Overall accuracy: 97.5%, which is excellent!

**Requirements**
MATLAB R2023a (or newer)

These toolboxes:

Signal Processing Toolbox
Wavelet Toolbox
Statistics and Machine Learning Toolbox
Predictive Maintenance Toolbox

This project was a great mix of signal processing, machine learning, and practical engineering.
It shows how you can take sensor data, process it intelligently, and teach a computer to recognize motor faults automatically.

**Author**
Name: Ahmed Razi Ullah
Email:ahmedrazi762@gmail.com
Linked In: www.linkedin.com/in/ahmed-razi-ullah-5b6b0a243

**Insights**
This project was a great mix of signal processing, machine learning, and practical engineering.
It shows how you can take sensor data, process it intelligently, and teach a computer to recognize motor faults automatically.