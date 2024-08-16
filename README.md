# Captcha-Reader-Utilizing-ML-Jetson-Nano-

{ "nbformat": 4, "nbformat\_minor": 0, "metadata": { "colab": { "provenance": \[\] }, "kernelspec": { "name": "python3", "display\_name": "Python 3" }, "language\_info": { "name": "python" } }, "cells": \[ { "cell\_type": "markdown", "source": \[ "## The Simple Captcha Reader for the Jetson Nano Utilizing Machine Learning" \], "metadata": { "id": "2br0V8Q6Gmws" } }, { "cell\_type": "markdown", "source": \[ "Welcome to the repository for the jetson nano captcha reasder program, before starting, please make sure that you have connected to your jetson nano, and you have connected it via visual studio code. The repository only covers the actions needed to be performe once the above portion is completed." \], "metadata": { "id": "jY\_uuylVGnLI" } }, { "cell\_type": "markdown", "source": \[ "The model's dataset was created with the help of the online captcha generator from https://usefoyer.com/tools/captcha-generator, the generator is capable of creating captchas of specific word or phrase.\\n" \], "metadata": { "id": "k\_hzjLqmsnBQ" } }, { "cell\_type": "markdown", "source": \[ "!\[0.png\](data:image/png;base64,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)" \], "metadata": { "id": "LizThbpJAjcE" } }, { "cell\_type": "markdown", "source": \[ "#To load in and properly use the data:\\n", "\\n", "\* Create folder captcha-reader\\n", "\* Create two folders inside captcha-reader named data, model\\n", "\* Create three folders inside data named train, test, val\\n", "\* Create three folders in train, test, val all named jetson, ice, nano\\n", "\* Create folder captcha-reader in model\\n", "\* add txt file labels.text and write jetson, ice, nano\\n", "\\n", "\\n", "The datasets of the captchas are going into their respective folders of jetson, ice, and nano, this helps to label the datasets.\\n", "\\n", "#The files would be listed here more comprehensively:\\n", "\\n", "\\n", "\* captcha-reader\\n", " \* data\\n", " \* test\\n", " \* jetson\\n", " \* nano\\n", " \* ice\\n", " \* train\\n", " \* jetson\\n", " \* nano\\n", " \* ice\\n", " \* val\\n", " \* jetson\\n", " \* nano\\n", " \* ice\\n", " \* model\\n", " \* captcha reader\\n", " \* labels.txt\\n", "\\n", "The datasets will go in the folders of test, train, and val, used respectively for the testing, trainging, and value processes.\\n", "\\n", "\\n", "\\n", "\\n", "\\n", "\\n", "\\n", "\\n", "\\n" \], "metadata": { "id": "QwKGr7OYBi5t" } }, { "cell\_type": "markdown", "source": \[ "To create large datasets, please use the code below to automate the process, use the request library to download set numbers of captchas." \], "metadata": { "id": "OR2Q9\_snAqao" } }, { "cell\_type": "code", "source": \[ "import requests\\n", "\\n", "folder = \\"jetson\\"\\n", "text = \\"jetson\\"\\n", "\\n", "for i in range(100):\\n", " url = 'https://usefoyer.com/ap/api/captcha?text='+text+'&type=text'\\n", " r = requests.get(url, allow\_redirects=True)\\n", " open(folder + \\"/\\" + str(i) +\\".png\\", 'wb').write(r.content)" \], "metadata": { "id": "D8DaVMsrA92e" }, "execution\_count": null, "outputs": \[\] }, { "cell\_type": "markdown", "source": \[ "For different words, please change the folder and text variable values." \], "metadata": { "id": "DqFaKmZkBOpD" } }, { "cell\_type": "markdown", "source": \[ "After dataset is complete proceed to create a new terminal in vscode. There, we will start the training process, before starting, the epoch is defaulted at 50 or 75, remember to change it to a higher number for better accuracy, the reccomendation is 150 to 200. The accuracy is not linear, sometimes less is best.\\n", "\\n", "Remember to put 70% of data into train, 15% into test, and 15% to val. From running the code, using 500 images in train, 200 in val, and 100 in test seemes to be the best option, providing a 88% accuracy." \], "metadata": { "id": "\_kIfzDLxBXTE" } }, { "cell\_type": "markdown", "source": \[ "#Step 1: Running the Docker\\n", "\\n", "Change directory to /home/nvidia by running the code below in your terminal\\n", "\\n", "\`\`\`\\n", "cd /home/nvidia\\n", "\`\`\`" \], "metadata": { "id": "eobywVpHGQwD" } }, { "cell\_type": "markdown", "source": \[ "If the result shows an error that is as follows\\n", "\\n", "\`\`\`\\n", "bash: cd: /home/nvidia: No such file or directory\\n", "\`\`\`\\n", "\\n" \], "metadata": { "id": "pDB-3LtlG8wa" } }, { "cell\_type": "markdown", "source": \[ "Try to find the folder in your vscode sidebar or look if the location is already in home or nvidia." \], "metadata": { "id": "s2r1f0T-HQ29" } }, { "cell\_type": "markdown", "source": \[ "Set your project to captcha-reader, so that the jetson nano can find the directory.\\n", "\\n", "\`\`\`\\n", "PROJECT=captcha-reader\\n", "\`\`\`" \], "metadata": { "id": "\_OOfyZFqHvig" } }, { "cell\_type": "markdown", "source": \[ "Now cd to jetson-inference, this will be the place where you will run the docker.\\n", "\\n", "\`\`\`\\n", "cd jetson-inference\\n", "./docker/run.sh --volume /home/nvidia/$PROJECT:/jetson-inference/$PROJECT\\n", "\`\`\`" \], "metadata": { "id": "rMYYB3A5H7\_m" } }, { "cell\_type": "markdown", "source": \[ "After running the docker, you are now ready to train your" \], "metadata": { "id": "00HhrTTaIWEb" } }, { "cell\_type": "markdown", "source": \[ "#Step 2: Training the Data\\n", "\\n", "Training the data will take a pretty long time depending on the amount of epochs you set it to or the amount of data used, but it is relatively easy to train.\\n", "\\n", "You must choose the amount of epochs to train the model, you can use the default of 50 to 75, but I reccomend using 150 to 200 to maximize accuracy.\\n", "\\n", "\`\`\`\\n", "--epochs=NumberOfEpochs\\n", "\`\`\`" \], "metadata": { "id": "hGZhEttzIeTj" } }, { "cell\_type": "markdown", "source": \[ "The to start training, you just need to run the below code.\\n", "\\n", "\`\`\`\\n", "python3 train.py --model-dir=/jetson-inference/captcha-reader/models/captcha-reader /jetson-inference/captcha-reader/data/captcha-reader\\n", "\`\`\`" \], "metadata": { "id": "alq0e2UyJuqx" } }, { "cell\_type": "markdown", "source": \[ "Now you have successfully started training your model." \], "metadata": { "id": "MpO2cFnuJ2ub" } } \] }
