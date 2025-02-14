<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Human Activity Recognition with ResNet-34</title>
</head>
<body>
    <h1>Human Activity Recognition with ResNet-34</h1>

    <p>This project demonstrates human activity recognition using a ResNet-34 model. It utilizes OpenCV for video processing and a pre-trained ResNet-34 model for activity classification.  The model is trained on the Kinetics dataset.</p>

    <h2>Table of Contents</h2>
    <ul>
        <li><a href="#introduction">Introduction</a></li>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#setup">Setup</a></li>
        <li><a href="#usage">Usage</a></li>
        <li><a href="#model-details">Model Details</a></li>
        <li><a href="#code-explanation">Code Explanation</a></li>
        <li><a href="#sleeping-detection">Sleeping Detection</a></li>
        <li><a href="#customization">Customization</a></li>
        <li><a href="#troubleshooting">Troubleshooting</a></li>
        <li><a href="#license">License</a></li>
    </ul>

    <h2 id="introduction">Introduction</h2>
    <p>This project aims to provide a simple and effective way to recognize human activities from video streams. It uses a pre-trained ResNet-34 model for feature extraction and classification, achieving real-time activity recognition.  The project is implemented in Python using OpenCV and the `cv2.dnn` module for deep learning inference.</p>

    <h2 id="prerequisites">Prerequisites</h2>
    <p>Before running this project, ensure you have the following installed:</p>
    <ul>
        <li>Python 3.6+</li>
        <li>OpenCV (cv2)</li>
        <li>NumPy</li>
    </ul>

    <p>You can install the necessary packages using pip:</p>
    <pre><code>
    pip install opencv-python numpy
    </code></pre>

    <h2 id="setup">Setup</h2>

    <ol>
        <li>Clone the repository to your local machine.</li>
        <li>Download the pre-trained ResNet-34 model (`resnet-34_kinetics.onnx`) and the action labels file (`action_recognition_kinetics.txt`).  These files should be placed in a `model/` directory within the project.  If the `model/` directory does not exist, create it.</li>
        <li>Ensure the file structure looks like this:
            <pre>
            <code>
            project_root/
            ├── recognise_human_activity.py
            ├── model/
            │   ├── resnet-34_kinetics.onnx
            │   └── action_recognition_kinetics.txt
            └── test/
                └── example1.mp4  (Optional: Example video)
            </code>
            </pre>
        </li>

    </ol>

    <h2 id="usage">Usage</h2>

    <p>To run the human activity recognition script, execute the following command in your terminal:</p>

    <pre><code>
    python recognise_human_activity.py
    </code></pre>

    <p>This will start the script, which will attempt to use the webcam by default.  To use a video file instead, ensure that the `VIDEO_PATH` parameter in the `Parameters` class is correctly set to the path of your video file.</p>

    <h2 id="model-details">Model Details</h2>

    <ul>
        <li><b>Model:</b> ResNet-34</li>
        <li><b>Dataset:</b> Kinetics</li>
        <li><b>Input Size:</b> 112x112</li>
        <li><b>Format:</b> ONNX</li>
    </ul>

    <p>The ResNet-34 model is loaded using `cv2.dnn.readNet()`. The input video frames are resized to 112x112 and preprocessed before being fed into the model. The model outputs a probability distribution over the possible actions, and the action with the highest probability is selected as the predicted activity.</p>

    <h2 id="code-explanation">Code Explanation</h2>

    <p>The Python script `recognise_human_activity.py` performs the following steps:</p>

    <ol>
        <li><b>Imports:</b> Imports necessary libraries such as `cv2`, `numpy`, and `collections.deque`.</li>
        <li><b>Parameters Class:</b> Defines a `Parameters` class to store important paths and constants, such as the paths to the model and class labels file, the input video path, sample duration, and sample size.</li>
        <li><b>Model Loading:</b> Loads the pre-trained ResNet-34 model using `cv2.dnn.readNet()`.</li>
        <li><b>Video Capture:</b> Accesses the video stream using `cv2.VideoCapture()`. It can either use a video file or the webcam.</li>
        <li><b>Frame Processing:</b>
            <ul>
                <li>Reads frames from the video stream in a loop.</li>
                <li>Resizes each frame to a fixed size (550x400).</li>
                <li>Appends the frame to a deque (`captures`). The deque stores a fixed number of recent frames (defined by `SAMPLE_DURATION`).</li>
                <li>Once the deque is full, it constructs an image blob from the frames using `cv2.dnn.blobFromImages()`. This blob is then preprocessed to match the input requirements of the ResNet-34 model.</li>
            </ul>
        </li>
        <li><b>Activity Recognition:</b>
            <ul>
                <li>Sets the input to the network using `net.setInput()`.</li>
                <li>Performs a forward pass through the model using `net.forward()` to obtain the output probabilities.</li>
                <li>Determines the predicted activity by finding the class label with the highest probability.</li>
                <li>Overlays the predicted activity label on the frame using `cv2.putText()`.</li>
                <li>Displays the resulting frame with the predicted activity using `cv2.imshow()`.</li>
            </ul>
        </li>
        <li><b>Sleeping Detection:</b> Implements a check to identify if the detected activity is "sleeping". If "sleeping" is detected, the script prints a warning message.  You can add functionality to play a sound or send a notification.</li>
        <li><b>Exiting:</b> Exits the loop when the 'q' key is pressed or when no more frames are available in the video stream.</li>
    </ol>

    <h2 id="sleeping-detection">Sleeping Detection</h2>

    <p>The script includes a basic implementation for detecting if a person is sleeping.  After predicting the activity, the code checks if the predicted label (converted to lowercase) contains the word "sleeping".</p>

    <pre><code>
    if "sleeping" in label.lower():
        print("Warning: The person is sleeping!")
        # You can add code here to play a beep sound, for example:
        # import winsound
        # winsound.Beep(1000, 1000) # Beep with a frequency of 1000 Hz for 1 second
    </code></pre>

    <p>You can extend this functionality to trigger actions like playing a beep sound (as shown in the commented-out code) or sending a notification.  To use the `winsound` module, you will need to install it (if it's not already installed) and ensure your operating system supports it (it is Windows-specific).</p>

    <h2 id="customization">Customization</h2>

    <p>You can customize the project by modifying the following parameters:</p>

    <ul>
        <li><b>`VIDEO_PATH` (in `Parameters` class):</b> Change this to the path of your video file or set it to `0` to use the webcam.</li>
        <li><b>`SAMPLE_DURATION` (in `Parameters` class):</b> Adjust this to control the number of frames used for each prediction.  A higher value may improve accuracy but will also increase processing time.</li>
        <li><b>`SAMPLE_SIZE` (in `Parameters` class):</b> Modify this to change the size of the input frames.</li>
        <li><b>Warning sound:</b> The current code includes commented-out code to play a warning sound when "sleeping" is detected. You can uncomment this code and modify the frequency and duration of the beep.  You can also replace this with other notification methods, such as sending an email or SMS.</li>
    </ul>

    <h2 id="troubleshooting">Troubleshooting</h2>

    <ul>
        <li><b>Error: `No module named cv2`</b>:  Make sure you have OpenCV installed (`pip install opencv-python`).</li>
        <li><b>Model loading errors</b>: Double-check the paths to the `resnet-34_kinetics.onnx` and `action_recognition_kinetics.txt` files in the `Parameters` class.</li>
        <li><b>Performance issues</b>:  If the script is running slowly, try reducing the `SAMPLE_SIZE` or `SAMPLE_DURATION`.</li>
        <li><b>Incorrect activity predictions</b>: The accuracy of the model depends on the quality of the video and the similarity of the activities to those in the Kinetics dataset. You may need to fine-tune the model on a custom dataset for better performance in specific scenarios.</li>
    </ul>

    <h2 id="license">License</h2>

    <p>This project is licensed under the MIT License - see the <a href="LICENSE">LICENSE</a> file for details.</p>

</body>
</html>
