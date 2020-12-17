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

For our project we have decided to employ a Long-Short Term Memory (LSTM) neural network.  This architecture falls under the umbrella of Recurrent Neural Networks (RNN).  LTSMs are able to use long-term dependencies to eliminate the memory issues of traditional RNNs (Olah, 2015).  Long-term memory provides context to the algorithm leading to more accurate outputs compared to other RNNs. This architecture is effective in handling time sequence data (Jeyakumar, Rec 2020 11 02) and thus will pair with our input of a sound signal’s magnitude over time.  The style of LSTM we will be using is the vanilla LSTM as referred to by (Greff, et al., 2017).  It is a baseline that performs well on various data inputs and, with modifications, curtails computational costs and the number of parameters without significant performance loss (Greff, et al., 2017).  These modifications include omitting the peephole connections and connecting the forget gates.  The reductions in run time and data overhead will scale down training time and lead to faster implementation. This is crucial given the short amount of time to finish our project.  There is also plenty of existing code for the LSTMs, such as (Jeyakumar, activity94/Activity-Recognition), (Karpathy, 2015), and (Phi, 2020).  These will be a foundation from which we build and tune our neural network.  

The LSTM will be coded, trained, and validated in Tensorflow using Google Colab.  The training data will include both pre-existing datasets from online sources and sensor data from the microphone on an Arduino Nano 33 Sense.  This microcontroller will also be the platform on which the LSTM will be run once it has been trained.  This will be achieved by converting the Tensorflow code into a Tensorflow Lite model that will be interpreted and loaded onto the Arduino.  The sole input of the model will be a data channel produced by the Arduino’s onboard microphone.  The information in this channel will include the magnitude in decibels of the environment and a timestamp.

The output of our LSTM will be the prediction of the volume of ambient sound in an environment.  This value will be transmitted to a smart phone application that will generate white noise to counteract any sounds that will disturb a sleeping individual.  The magnitude of the white noise will be proportional to the output value of the LSTM.


#### Data

#### Testing and Success

### Future Works

For future iterations of this project we’d like to make our model multi-class and predict the maximum magnitude of the audio signal.  A multiclass neural network would be more generalized and robust.  To use more than two classes, we’d need to collect quality audio data from a wide variety of sources.  In regards to predictions, we’d implement a regression model in addition to the classification currently used.  Predicting the loudest noise would allow our noise generating application to better mitigate the disturbance.

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

Farokhnezhad Afshar, P., Bahramnezhad, F., Asgari, P. & Shiri, M. Effect of White Noise on Sleep in Patients Admitted to a Coronary Care. Journal of caring sciences (2016). Available at: [https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4923834/](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4923834/). (Accessed: 3rd November 2020) 

Halperin, D. Environmental noise and sleep disturbances: A threat to health? Sleep science (Sao Paulo, Brazil) (2014). Available at: [https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4608916/](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4608916/). (Accessed: 3rd November 2020) 

Jeyakumar, J. V. Rec 2020 11 02. YouTube (2020). Available at: [https://www.youtube.com/watch?v=PKApMIci91Q](https://www.youtube.com/watch?v=PKApMIci91Q). (Accessed: 6th November 2020) 

Jeyakumar, J. V. vikranth94/Activity-Recognition. GitHub (2020). Available at: [https://github.com/vikranth94/Activity-Recognition/blob/master/existing_models.py](https://github.com/vikranth94/Activity-Recognition/blob/master/existing_models.py). (Accessed: 6th November 2020)

Karpathy, A. The Unreasonable Effectiveness of Recurrent Neural Networks (2015). Available at: [http://karpathy.github.io/2015/05/21/rnn-effectiveness/](http://karpathy.github.io/2015/05/21/rnn-effectiveness/). (Accessed: 6th November 2020)

K. Greff, R. K. Srivastava, J. Koutník, B. R. Steunebrink and J. Schmidhuber, "LSTM: A Search Space Odyssey," in IEEE Transactions on Neural Networks and Learning Systems, vol. 28, no. 10, pp. 2222-2232, Oct. 2017, doi: 10.1109/TNNLS.2016.2582924.

Olah, C. Understanding LSTM Networks. Understanding LSTM Networks -- colah's blog (2015). Available at: [https://colah.github.io/posts/2015-08-Understanding-LSTMs/](https://colah.github.io/posts/2015-08-Understanding-LSTMs/). (Accessed: 6th November 2020)

Phi, M. Illustrated Guide to LSTM's and GRU's: A step by step explanation. Medium (2020). Available at: [https://towardsdatascience.com/illustrated-guide-to-lstms-and-gru-s-a-step-by-step-explanation-44e9eb85bf21](https://towardsdatascience.com/illustrated-guide-to-lstms-and-gru-s-a-step-by-step-explanation-44e9eb85bf21). (Accessed: 6th November 2020)

Project IBID: Interaction Between Intelligent Devices. IBID Available at: [https://yifax.github.io/IBID/](https://yifax.github.io/IBID/).  

REM Sleep Tracker. REMtrack Available at: [https://ciankc.github.io/REMtrack/](https://ciankc.github.io/REMtrack/).  

Sayyad, R. A. How to Use Convolutional Neural Networks for Time Series Classification. (2020). Available at: [https://medium.com/@Rehan_Sayyad/how-to-use-convolutional-neural-networks-for-time-series-classification-80575131a474](https://medium.com/@Rehan_Sayyad/how-to-use-convolutional-neural-networks-for-time-series-classification-80575131a474).  

TensorFlow Lite inference. Available at: [https://www.tensorflow.org/lite/guide/inference](https://www.tensorflow.org/lite/guide/inference).  

Writer, T. E. Audio Classification Using CNN - Coding Example. (2019). Available at: [https://medium.com/x8-the-ai-community/audio-classification-using-cnn-coding-example-f9cbd272269e](https://medium.com/x8-the-ai-community/audio-classification-using-cnn-coding-example-f9cbd272269e).    
