## 🧠 Machine Learning Model

This project uses a deep learning model to classify 30-second sleep epochs into one of five stages based on features extracted from sensor data. The entire process, from training to on-device implementation, is designed to be lightweight and privacy-preserving.

---

### Dataset

The model was trained on the public **Sleep-EDF Expanded Dataset** available on PhysioNet.

* **Source:** [PhysioNet Sleep-EDF Expanded Database](https://physionet.org/content/sleep-edfx/1.0.0/)
* **Subjects:** Data from the `sleep-cassette` (SC) series, representing 20 healthy subjects over two nights, was used for training and validation.
* **Labels:** The data was labeled by experts into the following stages, which were mapped to integer classes for training:
    * `0`: Wake
    * `1`: Light Sleep (N1)
    * `2`: Light Sleep (N2)
    * `3`: Deep Sleep (N3/N4)
    * `4`: REM

---

### Features

The model is trained on features that can be reasonably proxied by a smartphone's sensors.

* **Primary Feature:** Since the training dataset does not contain accelerometer data, the **EMG (Electromyogram)** signal, which measures chin muscle activity, was used as a proxy for physical movement.
* **Input to Model:** The single input feature for the model is the **variance of the EMG signal**, calculated over a 30-second window. This value quantifies the amount of physical restlessness.

---

### Model Architecture

A sequential neural network was built and trained using **TensorFlow/Keras**. The architecture is simple and optimized for on-device performance.

| Layer Type      | Configuration                       | Purpose                                        |
| :-------------- | :---------------------------------- | :--------------------------------------------- |
| **Input** | Shape: (1,)                         | Accepts the single `emg_variance` feature      |
| **Dense** | 64 units, Activation: `ReLU`        | Learns initial patterns from the feature       |
| **Dropout** | Rate: 0.2                           | Reduces overfitting                            |
| **Dense** | 32 units, Activation: `ReLU`        | Learns more complex, higher-level patterns     |
| **Dense** | 5 units, Activation: `Softmax`      | Outputs a probability distribution for each of the 5 sleep stages |

---

### Training & Performance

The model was compiled and trained with the following configuration:

* **Optimizer:** `Adam`
* **Loss Function:** `sparse_categorical_crossentropy` (since labels are integers).
* **Metric:** `Accuracy`
* **Epochs:** 20
* **Performance:** The final model achieved a validation accuracy of approximately **XX.X%** on the held-out test set. *(Note: Replace XX.X% with your actual accuracy)*.

---

### On-Device Implementation

* **Format:** After training, the model was converted to the **TensorFlow Lite (`.tflite`)** format, a lightweight version optimized for mobile and embedded devices.
* **Usage:** The `sleep_model.tflite` file is included in the Android app's `assets` folder. All predictions are performed directly on the user's device using the TFLite Interpreter. This ensures that raw sensor data and sleep analysis results never leave the phone, guaranteeing user privacy.
