## Sleep Sound


### Project Topic

Mitigating the impact of sound on sleep.


### Team

Derek Wong and Grant Young

### Links

[Presentation](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4923834/)

[Github Repository](https://github.com/grant76/SleepSound)

### Proposal Abstract 

Environmental noise is one of the most common and detrimental factors to a horrible night’s rest and is an increasing problem in cities. The issue is environmental noise is impossible to avoid since it exists not only outside the home but inside as well. Instead of trying to avoid it, our project aims to use it to our advantage instead by generating white noise to block out these disturbing noises. By constantly monitoring the environment for sound and by using a neural network to classify certain sounds as disturbing noise, our system is able to playback white noise at an optimal sound level whenever these disturbing noises are detected in order to try to block them out and provide a better night’s rest. 


### Background

Most adults require seven to nine hours of sleep a day in order to properly function and be alert the very next day. The problem is that according to the CDC, a third of US adults reported getting less than the recommended amount of sleep. Sleep deprivation is linked to chronic diseases and conditions as well as mistakes both small and large. There are many factors that can lead to sleep deprivation, both internal and external factors, but it is difficult to pinpoint the exact cause. The focus of our project is to tackle a very trivial, but common factor that affects everyone - the environment, more specifically noise. 

Most people can agree that a silent environment is the best environment to sleep in, but the problem is not every environment can be silent. Environmental noise ranges from cars outside your window to conversations between people in your house. Environmental noise is tough to block out and it is even worse during sleep. Studies have shown that during sleep, environmental noise can negatively affect your sleep quality by reducing the number of deep sleep cycles and the amount of time spent in the deep sleep cycle. As a result, you end up spending more time in the more shallow sleep stages. This is due to biological responses, like increased adrenaline and cortisol levels as well as elevated heart rates (Halperin, 2014).  The worst part? You don’t even know it is happening until after the horrible night. 

In an article by Afshar et al, they conducted research about how white noise affects patients in the coronary care unit (Afshar, et al, 2016). The combination of conversations, telephones, alarms, electronic and medical devices, and other miscellaneous environmental sounds produced noise intensities that ranged from 59 to 90 dB. Using a self-rated sleep assessment test called the Pittsburgh Sleep Quality Index (PSQI), patients were asked to rate their sleep quality before and after the usage of a white noise machine. The PSQI scores have a range from 0 to 21. Before the usage of a white noise machine, the average score was 5.17 with a standard deviation of 1.66. After the usage of a white noise machine played at an intensity of around 40-50 dB, the average score increased to a staggering 11.23 with a standard deviation of 2.30. Because of these findings, the authors concluded the article by recommending the usage of a white noise machine in order to mask environmental noise and improve sleep. 


### Solution

Our focus isn’t to try to block environmental noise out, but instead use it to our advantage. Studies have shown that sound machines, like those that produce white noise, help people sleep better by moderating intermittent noise levels and providing a constant source of sound at an optimal sound level for a more peaceful and well-rested sleep. 

Our system would consist of a microcontroller that’s constantly monitoring the ambient noise using a microphone as well as outputting white noise using a speaker when disturbing noises are detected. Since the acoustics of a room differ from room to room, there will be a feedback loop in order to output the white noise at an optimal sound level. Whether a noise is considered disturbing or not will be up to a neural network. The user will be able to interact with our system using a mobile device via Bluetooth. The user will be able to adjust the system to their liking, for example changing the type of noise that’s played by our system. 

To predict disturbing noise we will use a neural network, which is a type of machine learning algorithm. This network will use ambient noise as an input to determine if a sound disturbance will occur and its magnitude.  The neural network will be deployed on an embedded microcontroller.  This provides flexibility in the environment where our machine learning model can be used.  The microcontroller will communicate the neural network’s prediction to a noise generator to counteract ambient sound throughout the night.


### Hardware

- Arduino Nano 33 Sennse BLE
- Oneplus 5

### Software

- Arduino IDE
- Edge Impulse Studio
- Android Studio

### Machine Learning

#### Machine Learning Model

We implemented a one dimensional convolutional neural network (CNN) created using Edge Impulse Studio (EIS) [Audio Classification, Resources].  The CNN was employed to classify time-series audio data into two categories, disturbance and no disturbance.  This identification allows for our compatible android application to modulate the magnitude at which it plays calming sounds.  EIS is a comprehensive suite of tools to build, train, and deploy neural networks.  This software also contains an integrated data collection and labeling system which makes this typically tedious task more efficient. 

The CNN was built using Keras in Tensorflow on EIS. Using EIS, the final model was converted to Arduino libraries using Tensorflow Lite.  

Originally our team had planned to use a Long-Short Term Memory (LSTM) model.  In the end, we chose a CNN due to Edge Impulse Studio’s audio classification tools.  They are specifically tuned to CNNs and produced excellent results in our binary classification.  Also, the system’s ease of use was hard to overlook.  The process from data acquisition to deployment was nearly seamless.

#### Data

Audio data was collected on the Arduino Nano using the acquisition tools available in Edge Impulse Studio (EIS).  The Arduino uses a pulse density modulation (PDM) scheme to capture audio data.  PDM keeps noise low in the desired frequency band, thus improving signal integrity (Kite).  The EIS tools were intuitive and efficient.  They provided a GUI to interface directly with the Arduino that allowed us to easily capture and label the data.  It also handled the reshaping of data to be input into our machine learning model.

In regards to a no disturbance, audio was recorded in quiet rooms with a lack of abrupt sounds.

For the disturbance data, we sought out disruptive sounds around bedroom areas.  Dissonant noises produced data that was most distinguishable from an ideal sleeping environment.  These disturbances included, but were not limited to, human voices, items falling to the ground, and doors being shut.  Consonant sounds, such as running appliances, were much more similar to a quiet surroundings and thus made up a smaller proportion of our dataset. 

All of our labelled audio data is available on the github repository linked above.

#### Testing and Success

Once our data was collected, the next goal was to determine when a noise of disturbing levels had occurred.  The CNN was trained and produced an astounding 99% accuracy on the validation data.  Our self-imposed success metric for testing was to classify with at least 90% accuracy.  The model passed with flying colors with an accuracy of 98%.

In total we recorded just about 70 minutes of audio, with sixty percent disturbance and the rest no disturbance.  This data was split 80-20-20 for training, validation, and testing. The model uses a batch size of 256, the Adam optimizer, and a binary cross entropy loss function.  It was trained over two-hundred epochs.  This CNN produced an accuracy of 99.6% with a confusion matrix [(99.8% 0.2%) (0.8% 99.2%)].  This model also performed well in testing.  It produced an accuracy of 98.6% on 12 minutes of audio.

From our point of view, these results may not be fully indicative of real world conditions.  The data collected was from a limited number of sources that were available to us given the current pandemic.  These sources include human voices, household appliances, dropped items, loud movement, and silent rooms.  The model may have been too finely tuned to the recording  surroundings and may not perform as well in new environments.

### Arduino Implementation

The machine learning model is converted to a Tensorflow Lite model and housed on the Arduino Nano 33 BLE Sense board. The Arduino uses the onboard digital microphone to record sound from the environment, classify the sound, and relay this information to the application on the Android phone where it adjusts the volume of the white noise accordingly. In order to connect the Arduino and the Android phone, we decided to use Bluetooth Low Energy, or BLE. The reason for using this is because the Arduino also has a BLE module onboard so it requires less hardware and realistically, the Arduino would be a standalone device placed near a sound source and is meant to be a set-it-and-forget-it sort of device so energy conservation is also a consideration. In a BLE relationship, the Arduino would be acting as the peripheral where the Android phone would be acting as the central. We used Arduino’s built-in BLE library in order to create our “Sleep Sound Service”. We did this by giving our device a name to search for, a universally unique identifier for the service, and a universally unique identifier for the transfer characteristic. The characteristic is initialized as a “BLEFloatCharacteristic” because we would be using this characteristic to send data of type float to the Android application. As an added feature, in the event that the Android application connects to the Arduino, the built-in LED on the Arduino should light up, signifying a connection. If the light is off, no connection is made or the user disconnected from the Arduino. Our Arduino code to connect to an Android application using BLE is a modified version of C. Thomas Brittain’s code (Ladvien, 2020).

### Android Application Implementation

The purpose of the Android application is to basically connect the user to the machine learning model on the Arduino. The Arduino does all of the heavy lifting by recording the sound in the environment, running it through the machine learning model, and sending data to the phone where that data is used to adjust the volume of the sound played from the phone. All the user has to do is wait for the application to find the Arduino, prompt a connection, choose their preferred sound to play, hit play, and go to sleep while everything else is done for them. 

The operation of the application is best illustrated using a finite state machine. There’s a total of 5 states: scan (default), found, connected, play, and time-out.

![Figure 1 - Sleep Sound application interface default: scan state](https://user-images.githubusercontent.com/42701588/102657332-5840b680-412a-11eb-9085-1276e0f6efcf.png)

Figure 1 above shows the application’s default screen. Although not visible, the application is currently scanning for nearby pairable devices, specifically the Sleep Sound Arduino. Within the code, the UUID of the Sleep Sound Arduino specified in the Arduino sketch is the only UUID the application is allowed to connect to, so although there may be other devices nearby, it will not recognize them. All the buttons and sound selection are disabled so the user doesn’t really have to do much but wait.

![Figure 2 - Sleep Sound application interface: time-out state](https://user-images.githubusercontent.com/42701588/102657533-ace43180-412a-11eb-8c84-a6474bad8f46.png)

In the event that the application cannot find the Sleep Sound Arduino, the bluetooth scanning will time-out, in which case Figure 2 above shows what happens to the application. In bright red text, the application shows the user that it could not find the Arduino and therefore could not connect to it. At this point, the user would have to do some troubleshooting on their own to figure out why, but all the user has to do to rescan would be to restart the application. 

![Figure 3 - Sleep Sound application interface: found state](https://user-images.githubusercontent.com/42701588/102657583-c71e0f80-412a-11eb-8f66-a08e031a0397.png)

On the opposite side of the spectrum, if the application is able to find the Sleep Sound Arduino, the “Arduino Status” changes to “Found!” in bright green text. The application doesn’t automatically connect to the Arduino so the user is given the responsibility of manually connecting by simply pressing the “Connect” button, as shown in Figure 3 above. 

![Figure 4 - Sleep Sound application interface: connected state](https://user-images.githubusercontent.com/42701588/102657653-e4eb7480-412a-11eb-9982-e18ed9cabbbf.png)

Once the user prompts the connection to the Arduino, the “Connection Status” changes to “Connected!” in bright green text. At this point, the sound library is enabled so the user can choose their preferred sound and prompt “Play” or disconnect from the application altogether. Figure 4 above shows this. The sounds in the sound library were sourced from YouTube and converted to mp3 (Gentle Ocean Waves, 2015)(therelaxedguy, 2014). From there, the mp3 files were stored in a specific location on the phone where the application had access to in order to play the sounds.  

![Figure 5 - Sleep Sound application interface: play state](https://user-images.githubusercontent.com/42701588/102657700-fc2a6200-412a-11eb-9e69-4ef91c48f096.png)

Once the user prompts the “Play”, the sound library is once again disabled but this time the “Pause” button is enabled. If the user hits “Pause”, it will return to the connected state where the user can choose their sound again. At this point, their sound selection should be playing through their phone speaker or through a connected bluetooth connection, if their phone can support multiple bluetooth devices. The user can choose to disconnect from the Arduino anytime in the connected and play state. If they do, they will simply be returned to the scan state, or default, and the process starts over again. 

The Android application is built on Android 10 API level 29 using Java in Android Studio. The primary test device is a Oneplus 5 running a proprietary OS called OxygenOS version 10.0.1, which is based off of Android 10. The basis of the code our application was built on top of was thanks to Maty (Maty, 2020).


### Future Works

For future iterations of this project we’d like to make our model multi-class and predict the maximum magnitude of the audio signal.  A multiclass neural network would be more generalized and robust.  To use more than two classes, we’d need to collect quality audio data from a wide variety of sources.  In regards to predictions, we’d implement a regression model in addition to the classification currently used.  Predicting the loudest noise would allow our noise generating application to better mitigate the disturbance.

Another consideration for future iterations of this project would be to try to incorporate other non-invasive hardware sensors as well to help further improve sleep. Sound is only one of many factors that affect a person’s quality of sleep. There are other factors in the environment that we can control, such as the temperature, humidity, and brightness. Each factor is a challenge of its own to try to monitor and control, but would ultimately lead to the greatest possible quality of sleep. 

### Timeline and Deliverables

Week 5:  

- Write software for neural network model.  
- Collect training data.
- Research hardware system design.

Week 6:  

- Gather more data using Arduino.  
- Continue writing software for neural network.
- Write software for hardware integration.

Week 7:  

- Train and tune the network using gathered data.
- Test model for accuracy. 
- Write software for human-computer interaction.
  
Week 8:  

- Continue to train, tune, and test as necessary.  
- Convert the model and deploy it on our embedded system.

Week 9:  

- Test the whole system:
- CNN model deployment on Arduino.
- Hardware integration.
- Human-computer interaction integration.

Week 10:  

- Finalize system. 
- Submit documentation.


#### Resources

- [Edge Impulse Studio](https://studio.edgeimpulse.com/)
- [Audio Classification](https://docs.edgeimpulse.com/docs/audio-classification)
- [Collecting Data through Edge Impulse Studio using Arduino](https://docs.edgeimpulse.com/docs/arduino-nano-33-ble-sense)


#### References

ArduinoBLE. Arduino Available at: [https://www.arduino.cc/en/Reference/ArduinoBLE](https://www.arduino.cc/en/Reference/ArduinoBLE). (Accessed: 18th December 2020)

Bluetooth low energy overview &nbsp;: &nbsp; Android Developers. Android Developers Available at: [https://developer.android.com/guide/topics/connectivity/bluetooth-le](https://developer.android.com/guide/topics/connectivity/bluetooth-le). (Accessed: 18th December 2020) 

Farokhnezhad Afshar, P., Bahramnezhad, F., Asgari, P. & Shiri, M. Effect of White Noise on Sleep in Patients Admitted to a Coronary Care. Journal of caring sciences (2016). Available at: [https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4923834/](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4923834/). (Accessed: 3rd November 2020) 

Gentle Ocean Waves 20 Minutes Meditation Relaxation Sleep Better Reduce Stress. YouTube (2015). Available at: [https://www.youtube.com/watch?v=R6Dv0h9DdBc](https://www.youtube.com/watch?v=R6Dv0h9DdBc). (Accessed: 18th December 2020)

Getting Started with Bluetooth LE on the Arduino Nano 33 Sense. Ladvien's Lab (2020). Available at: [https://ladvien.com/arduino-nano-33-bluetooth-low-energy-setup/](https://ladvien.com/arduino-nano-33-bluetooth-low-energy-setup/). (Accessed: 18th December 2020) 

Halperin, D. Environmental noise and sleep disturbances: A threat to health? Sleep science (Sao Paulo, Brazil) (2014). Available at: [https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4608916/](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4608916/). (Accessed: 3rd November 2020) 

Jeyakumar, J. V. Rec 2020 11 02. YouTube (2020). Available at: [https://www.youtube.com/watch?v=PKApMIci91Q](https://www.youtube.com/watch?v=PKApMIci91Q). (Accessed: 6th November 2020) 

Jeyakumar, J. V. vikranth94/Activity-Recognition. GitHub (2020). Available at: [https://github.com/vikranth94/Activity-Recognition/blob/master/existing_models.py](https://github.com/vikranth94/Activity-Recognition/blob/master/existing_models.py). (Accessed: 6th November 2020)

Karpathy, A. The Unreasonable Effectiveness of Recurrent Neural Networks (2015). Available at: [http://karpathy.github.io/2015/05/21/rnn-effectiveness/](http://karpathy.github.io/2015/05/21/rnn-effectiveness/). (Accessed: 6th November 2020)

K. Greff, R. K. Srivastava, J. Koutník, B. R. Steunebrink and J. Schmidhuber, "LSTM: A Search Space Odyssey," in IEEE Transactions on Neural Networks and Learning Systems, vol. 28, no. 10, pp. 2222-2232, Oct. 2017, doi: 10.1109/TNNLS.2016.2582924.

Kite, T. Understanding PDM Digital Audio. Available at: [http://users.ece.utexas.edu/~bevans/courses/rtdsp/lectures/10_Data_Conversion/AP_Understanding_PDM_Digital_Audio.pdf](http://users.ece.utexas.edu/~bevans/courses/rtdsp/lectures/10_Data_Conversion/AP_Understanding_PDM_Digital_Audio.pdf).  

Maty. BlueCArd - part 6 - Controlling the Arduino Nano Bluetooth module from Android device. Techblog (2020). Available at: [https://www.thinker-talk.com/post/bluecard-part-6-controlling-the-arduino-nano-bluetooth-module-from-android-device](https://www.thinker-talk.com/post/bluecard-part-6-controlling-the-arduino-nano-bluetooth-module-from-android-device). (Accessed: 18th December 2020) 

Olah, C. Understanding LSTM Networks. Understanding LSTM Networks -- colah's blog (2015). Available at: [https://colah.github.io/posts/2015-08-Understanding-LSTMs/](https://colah.github.io/posts/2015-08-Understanding-LSTMs/). (Accessed: 6th November 2020)

Phi, M. Illustrated Guide to LSTM's and GRU's: A step by step explanation. Medium (2020). Available at: [https://towardsdatascience.com/illustrated-guide-to-lstms-and-gru-s-a-step-by-step-explanation-44e9eb85bf21](https://towardsdatascience.com/illustrated-guide-to-lstms-and-gru-s-a-step-by-step-explanation-44e9eb85bf21). (Accessed: 6th November 2020)

Project IBID: Interaction Between Intelligent Devices. IBID Available at: [https://yifax.github.io/IBID/](https://yifax.github.io/IBID/).  

REM Sleep Tracker. REMtrack Available at: [https://ciankc.github.io/REMtrack/](https://ciankc.github.io/REMtrack/).  

Sayyad, R. A. How to Use Convolutional Neural Networks for Time Series Classification. (2020). Available at: [https://medium.com/@Rehan_Sayyad/how-to-use-convolutional-neural-networks-for-time-series-classification-80575131a474](https://medium.com/@Rehan_Sayyad/how-to-use-convolutional-neural-networks-for-time-series-classification-80575131a474).  

TensorFlow Lite inference. Available at: [https://www.tensorflow.org/lite/guide/inference](https://www.tensorflow.org/lite/guide/inference).  

therelaxedguy. 30 MINUTES Rain Sounds (no music or thunder), Light Rain for Sleep, Relaxing, Meditate, Study, Yoga. YouTube (2014). Available at: [https://www.youtube.com/watch?v=j9nhecEWMuE](https://www.youtube.com/watch?v=j9nhecEWMuE). (Accessed: 18th December 2020) 

Writer, T. E. Audio Classification Using CNN - Coding Example. (2019). Available at: [https://medium.com/x8-the-ai-community/audio-classification-using-cnn-coding-example-f9cbd272269e](https://medium.com/x8-the-ai-community/audio-classification-using-cnn-coding-example-f9cbd272269e).    
