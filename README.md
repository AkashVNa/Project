# Project
Identification of network attacks on IoT devices

**Table of Contents**:


1. [Introduction](#intro)
2. [Project Goal](#goal)
3. [Data Collection](#datacollect)
4. [Data Story](#story)
5. [Data Loading](#dataloading)
6. [Dataset Analysis](#dataanalysis)
7. [Data Cleaning](#cleaning)
8. [Feature Selection](#featselect)
9. [Outlier Handling](#outhand)
10. [Data Encoding](#encoding)
11. [Data Sampling](#datsam)
12. [Feature Engineering](#feateng)
13. [Model Building](#modbuild)
14. [Pipelining the model](#pipe)
15. [Saving the model](#save)

<a name="intro"></a>**Introduction**:
The Internet of Things (IoT) refers to a network of physical devices, vehicles, appliances, and other physical objects that are embedded with sensors, software, and network connectivity, allowing them to collect and share data. IoT devices—also known as “smart objects”—can range from simple “smart home” devices like smart thermostats, to wearables like smartwatches and RFID-enabled clothing, to complex industrial machinery and transportation systems.[1](https://www.ibm.com/topics/internet-of-things)
Forrester Research concluded in its "The State of IoT Security, 2023" report that IoT devices were the most reported target for external attacks; they were attacked more than either mobile devices or computers. Hackers scan networks for devices and known vulnerabilities and increasingly use nonstandard ports to get network access. Once they have device access, it's easier to avoid detection through fileless malware or software memory on the device.[2](https://www.techtarget.com/iotagenda/tip/5-IoT-security-threats-to-prioritize)
Owing to the increasing presence of IoT devices and the risk of compromising personal data to hackers, it is necessary to enforce robust mechanisms which defends against these attacks.By incorporating data from IoT devices such as ThingSpeak-LED, Wipro-Bulb, and MQTT-Temp, as well as simulated attack scenarios involving Brute-Force SSH attacks, DDoS attacks using Hping and Slowloris, and Nmap patterns the nature of network traffic is analysed using the Zeek network monitoring tool and the Flowmeter plugin.Using these data, a model building is attempted to effectively classify the nature of network interaction going on which leads to the classification of type of attack faced by the IoT devices.

<a name="goal"></a>**Project Goal**:
The project aims to analyse the changes in the network parameters caused due to attacks on IoT devices. This helps to neutralize threats efficiently by aiding the Intrusion Detection Systems in effectively identifying the type of threat.
A model building is attempted which takes network parameters as features to make a classification of the type of attack from among some known attack types.

<a name="datacollect"></a>**Data Collection:**
The data is collected from a secondary source that is UCIrvine Machine Learning Repository. The [dataset](https://archive.ics.uci.edu/dataset/942/rt-iot2022) was donated to the repository and it appeared in an introductory [paper](semanticscholar.org/paper/Quantized-autoencoder-(QAE)-intrusion-detection-for-Sharmila-Nagapadma/753f6ede01b4acaa325e302c38f1e0c1ade74f5b) published in Cybersecurity. The citation for the same is given below.
S., B. & Nagapadma, R. (2023). RT-IoT2022  [Dataset]. UCI Machine Learning Repository. https://doi.org/10.24432/C5P338

<a name="story"></a>**Data Story:**
The dataset encompasses both normal and adversarial network behaviors, providing a general representation of real-world scenarios.Incorporating data from IoT devices such as ThingSpeak-LED, Wipro-Bulb, and MQTT-Temp, as well as simulated attack scenarios involving Brute-Force SSH attacks, DDoS attacks using Hping and Slowloris, and Nmap patterns, RT-IoT2022 offers a detailed perspective on the complex nature of network traffic. The bidirectional attributes of network traffic are meticulously captured using the Zeek network monitoring tool and the Flowmeter plugin.Infrastructure consists of two parts, namely IoT victim devices and IoT attacker devices, both connected through a router. We collect the network traffic through a router using Wireshark, which is an open-source monitoring tool for network traffic that helps extract traces and convert them into a PCAP file.The attacking infrastructure includes 50 machines, and the victim organization has 5 departments and includes 420 machines and 30 servers. The dataset includes the captures network traffic and system logs of each machine, along with 80 features extracted from the captured traffic.It includes 9 different attack scenarios: DOS_SYN_Hping,, ARP_poisioning, NMAP_UDP_SCAN,
NMAP_XMAS_TREE_SCAN, NMAP_OS_DETECTION, NMAP_TCP_scan, DDOS_Slowloris, Metasploit_Brute_Force_SSH, NMAP_FIN_SCAN and 3 normal pattern MQTT, Thing_speak and Wipro_bulb_Dataset.[3](https://www.kaggle.com/datasets/supplejade/rt-iot2022real-time-internet-of-things)

<a name="dataloading"></a>**DATA LOADING**:
The data is in csv format and the data is loaded as a Dataframe using pandas library.

<a name="dataanalysis"></a>**DATASET ANALYSIS**:
The dataset consist of 85 columns and 123117 rows. It consist of 3 categorical columns and the remaining numerical columns.
Null values are not present in any of the columns but in the service column there is a class named '-' which might or mightnot indicate an unknown value.
The categorical columns are heavily biased.

![image](https://github.com/user-attachments/assets/f9cc11e3-bf04-444c-b746-0f3bdee3a19d)


<a name="cleaning"></a>**DATA CLEANING**:
An index column is repeated which is removed in cleaning phase itself.
The duplicates are removed.

<a name="featselect"></a>**FEATURE SELECTION**:
Two columns are indication of id of devices used so they are removed. They doesnot provide any value in model building.

#<a name="outhand"></a> **OUTLIER HANDLING**:
Numerical data of the dataset consist of huge outlier presence.

![image](https://github.com/user-attachments/assets/891db72d-948c-44bd-9737-0bc3f07ecf2c)
![image](https://github.com/user-attachments/assets/1d4f371e-d8fc-4db8-9330-1fda5ee3fcc7)
![image](https://github.com/user-attachments/assets/ab926b0f-0948-48d0-a7e7-1eda0c893a0f)


The standard deviations and range also seem to vary with high values.

![image](https://github.com/user-attachments/assets/f95e168c-6bc4-437b-9b93-e45e0f735c91)

![image](https://github.com/user-attachments/assets/8479d22f-e8c4-4ef6-bb0f-e76ca160c45a)

The numerical data is then made to undergo InterQuartileRange normalisation which decreased the outlier presence.

![image](https://github.com/user-attachments/assets/12f5574b-5295-4e32-8953-724ffa0a5874)
![image](https://github.com/user-attachments/assets/9c0b6975-04de-4e79-98c1-0dd8db1cd5c4)
![image](https://github.com/user-attachments/assets/84af92a0-41de-4649-abf1-774f8b72ada4)



To further reduce the outliers, the numerical data is made to undergo logarithmic transformation.

![image](https://github.com/user-attachments/assets/c5241447-1907-490b-bb49-57a02f00366b)
![image](https://github.com/user-attachments/assets/aa8baf95-8163-427a-853c-25459bafca20)
![image](https://github.com/user-attachments/assets/072815f8-368c-4016-9133-d5d69d1a2105)



<a name="encoding"></a>**DATA ENCODING**:
There are two categorical columns which are encoded using one hot encoder.

<a name="datsam"></a>**DATA SAMPLING**:
The target class is heavily biased. The most dominant class has 90089 entries and least occurring class has 28 entries.

![image](https://github.com/user-attachments/assets/12eec2ba-5cfa-4755-ae19-85bb118618bf)

Five classes having low presence is removed from the data and the entries in the most dominant class is reduced to 10000 values by random selection.
The remaining classes are then oversampled to reach 10000 entries each.

![image](https://github.com/user-attachments/assets/9eee5958-1556-4232-b152-4ee5d5a21e56)


<a name="feateng"></a>**FEATURE ENGINEERING**:
The encoded dataset is then checked for collinearity and was consisting of highly collinear features. Some of them are plotted.

![image](https://github.com/user-attachments/assets/a05ddca8-5ba9-472c-9c6e-4b51bd1abc9b)
![image](https://github.com/user-attachments/assets/b5f3fa72-4b71-4997-815b-12921db9970b)
![image](https://github.com/user-attachments/assets/7177e9ca-e6f0-4c0a-9a29-d0f2c07ac2d1)
![image](https://github.com/user-attachments/assets/2cc449d6-97e5-4847-b977-fe97c632318f)
![image](https://github.com/user-attachments/assets/56f0592e-cf6f-4d09-8ee2-f0c73796808e)



The correlation matrix also shows collinearity.

![image](https://github.com/user-attachments/assets/76f6ce81-83b4-40c3-89c6-49d8a63dfb4d)

Due to the higher collinearity, dimensional reduction is carried out and the dimensions are chosen in such a way that 99.99% of cumulative explained variance ratio is achieved.

![image](https://github.com/user-attachments/assets/3d7cc53d-9e6b-47b4-9118-8c2a5af9f167)

The dimensions are fixed to be of 12 numbers and collinearity is again checked using correlation matrix.

![image](https://github.com/user-attachments/assets/b5e5997c-c21a-4858-8202-db96eb5b108c)


<a name="modbuild"></a>**MODEL BUILDING**:
Five models are built with different algorithms which are Gaussian Naive Bayes, Logistic Regression, KNeighbors Classifier, Random Forest Classifier, Support Vector Classifier.
Initially system defined parameters are used to train the models.
Then hyperparameter tuning is carried out using grid search and models are built using new parameters.

![download](https://github.com/user-attachments/assets/c26181a3-9757-48d3-9571-6ae8f71fe50d)
![download](https://github.com/user-attachments/assets/d62da916-cb67-40de-9a61-9a1f5013d4f6)
![download](https://github.com/user-attachments/assets/ddebe468-2774-46ce-9354-f162888b2ced)
![download](https://github.com/user-attachments/assets/eb2d528e-5fc7-42d3-bbc4-ed8846038dc6)
![download](https://github.com/user-attachments/assets/4c7e8f76-a097-4b93-96d4-712661ab32d2)



The hyperparameters seem to improve many metrics but the precision seem to be reduced after this tuning.
Random forest Classifier seem to be having nearly perfect training vs testing accuracy score which highlights the good fitting. Moreover this model shows best accuracy score and best precision too.
Random Forest Classifier is selected as the preferred model for the problem.

<a name="pipe"></a>**PIPELINING THE MODEL**:
Following pipelining steps are implemented.
Numerical columns--->IQR normalisation--->logarithmic transformation--->to form new dataset
Categorical columns--->One hot encoding--->to form new dataset
New dataset--->PCA transformation--->Standard Scaling--->Random Forest Classifier
The pipeline is trained with data and prediction is checked. Data after sampling process is converted to the original dataset form by reversing the encoding and reversing the logarthmic transformation.

![download](https://github.com/user-attachments/assets/8e9ff421-70bc-4969-9578-9e53b0481e63)


<a name="save"></a>**SAVING THE MODEL**
The pipeline model is saved to a pickle file.
