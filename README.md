# Captcha-Reader-Utilizing-ML-Jetson-Nano-
The Simple Captcha Reader for the Jetson Nano Utilizing Machine Learning
Welcome to the repository for the jetson nano captcha reasder program, before starting, please make sure that you have connected to your jetson nano, and you have connected it via visual studio code. The repository only covers the actions needed to be performe once the above portion is completed.

The model's dataset was created with the help of the online captcha generator from https://usefoyer.com/tools/captcha-generator, the generator is capable of creating captchas of specific word or phrase.

0.png

To load in and properly use the data:
Create folder captcha-reader
Create two folders inside captcha-reader named data, model
Create three folders inside data named train, test, val
Create three folders in train, test, val all named jetson, ice, nano
Create folder captcha-reader in model
add txt file labels.text and write jetson, ice, nano
The datasets of the captchas are going into their respective folders of jetson, ice, and nano, this helps to label the datasets.

The files would be listed here more comprehensively:
captcha-reader
data
test
jetson
nano
ice
train
jetson
nano
ice
val
jetson
nano
ice
model * captcha reader
labels.txt
The datasets will go in the folders of test, train, and val, used respectively for the testing, trainging, and value processes.

To create large datasets, please use the code below to automate the process, use the request library to download set numbers of captchas.


import requests

folder = "jetson"
text = "jetson"

for i in range(100):
    url = 'https://usefoyer.com/ap/api/captcha?text='+text+'&type=text'
    r = requests.get(url, allow_redirects=True)
    open(folder + "/" + str(i) +".png", 'wb').write(r.content)
     
For different words, please change the folder and text variable values.

After dataset is complete proceed to create a new terminal in vscode. There, we will start the training process, before starting, the epoch is defaulted at 50 or 75, remember to change it to a higher number for better accuracy, the reccomendation is 150 to 200. The accuracy is not linear, sometimes less is best.

Remember to put 70% of data into train, 15% into test, and 15% to val. From running the code, using 500 images in train, 200 in val, and 100 in test seemes to be the best option, providing a 88% accuracy.

Step 1: Running the Docker
Change directory to /home/nvidia by running the code below in your terminal

cd /home/nvidia
If the result shows an error that is as follows

bash: cd: /home/nvidia: No such file or directory
Try to find the folder in your vscode sidebar or look if the location is already in home or nvidia.

Set your project to captcha-reader, so that the jetson nano can find the directory.

PROJECT=captcha-reader
Now cd to jetson-inference, this will be the place where you will run the docker.

cd jetson-inference
./docker/run.sh --volume /home/nvidia/$PROJECT:/jetson-inference/$PROJECT
After running the docker, you are now ready to train your

Step 2: Training the Data
Training the data will take a pretty long time depending on the amount of epochs you set it to or the amount of data used, but it is relatively easy to train.

You must choose the amount of epochs to train the model, you can use the default of 50 to 75, but I reccomend using 150 to 200 to maximize accuracy.

--epochs=NumberOfEpochs
The to start training, you just need to run the below code.

python3 train.py --model-dir=/jetson-inference/captcha-reader/models/captcha-reader /jetson-inference/captcha-reader/data/captcha-reader
Now you have successfully started training your model.
