# Lab 3: Threat modelling

In the context of application security, threat modeling is a structured, repeatable process used to gain actionable insights into the security characteristics of a particular system. It involves modeling a system from a security perspective, identifying applicable threats based on this model, and determining responses to these threats. Threat modeling analyzes a system from an adversarial perspective, focusing on ways in which an attacker can exploit a system.

Threat modeling is ideally performed early in the SDLC, such as during the design phase. Moreover, it is not something that is performed once and never again. A threat model is something that should be maintained, updated and refined alongside the system. Ideally, threat modeling should be integrated seamlessly into a team's normal SDLC process; it should be treated as standard and necessary step in the process, not an add-on.

According to the [Threat Model Manifesto](https://www.threatmodelingmanifesto.org/), the threat modeling process should answer the following four questions:

1. What are we working on?
2. What can go wrong?
3. What are we going to do about it?
4. Did we do a good enough job?

From: [https://cheatsheetseries.owasp.org/cheatsheets/Threat\_Modeling\_Cheat\_Sheet.html](https://cheatsheetseries.owasp.org/cheatsheets/Threat\_Modeling\_Cheat\_Sheet.html)

To do the threat modelling, we are going to use the OWASP Threat Dragon that allows the drawing of a Data Flow Diagram of the system we are modeling and then applying threats by using the STRIDE model.&#x20;

### Installing Threat Dragon

There is a choice, you can run this on the Parrot VM or you can install Threat Dragon on your laptop.&#x20;

To run on the VM:

```
cd /opt/threat-dragon
sudo npm start
```

Then open a browser window and go to&#x20;

http://127.0.0.1:8080

To run on your own laptop, follow the instructions:

Install OWASP Threat Dragon from the [releast page](https://github.com/OWASP/threat-dragon/releases) on GitHub.

### Threat modelling the Autonomous Super System Asset Management  (ASSAM)

Actually, we aren't going to threat model the entire system, we are just going to look at the development of a model that goes into the system. As a result, we will cover some of the potential threats and mitigations when developing and running such a system.&#x20;

Development Description

#### Creating Training Data and Training Model

Data is collected from a range of different sources (databases, spreadsheets, reports) about assets, characteristics of their operational environments, and their maintenance/replacement over their lifetimes.&#x20;

The data is collected by specific users tasked with this responsibility (Data Collectors).

This data is sanitised by Data Scientists to ensure it does not contain personal or confidential information and then a portion of it has been sent to a third party to be classified. Classifiers label the data and return it to the Data Scientists.

The data scientists are involved in this process of peparation of the training data set and the end result then goes into a process of training of new models. &#x20;

The training involves the use of Jupyter Notebooks utilising Pythong ML libraries and a Jupyter Server. Python code is written however to do the training of the model and this is run on AWS. Training data and outputs are stored in AWS S3.&#x20;

#### ASSAM Application

The application utilises the ASSAM model by interfacing with the ChatGPT API. This allows users to create english language queries to the model regarding specific assets.&#x20;

A user logs into the ASSAM app through their account in a browser and will have a range of functions  provided by a backend server functionality that has been written in Python Django that in turn uses a MySQL database.&#x20;

#### Create the Data Flow Diagram

The purpose of this exercise is to familiarise yourself with Threat Dragon and the process you would go through in constructing Data Flow Diagrams and the associated Threat Model.

A starting DFD has been created for the ASSAM Threat Model. You can download the following Threat Dragon file and open it to get started:

{% file src=".gitbook/assets/open-asset.json" %}

Taking the stores, processes and actors as a starting point, connect these up and add the appropriate threat boundaries and threats to the various components.&#x20;

Again for the purposes of this exercise, we will consider the following threats to the AI/ML system (Taken from [Microsoft AI/ML Security](https://learn.microsoft.com/en-us/security/engineering/failure-modes-in-machine-learning)):

| Threat Number | Threat                                          |                                                                                                                                                  |
| ------------- | ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1             | Perturbation attack                             | Attacker modifies the query to get appropriate response                                                                                          |
| 2             | Poisoning attack                                | Attacker contaminates the training phase of ML systems to get intended result                                                                    |
| 3             | Model Inversion                                 | Attacker recovers the secret features used in the model by through careful queries                                                               |
| 4             | Membership Inference                            | Attacker can infer if a given data record was part of the model’s training dataset or not                                                        |
| 5             | Model Stealing                                  | Attacker is able to recover the model through carefully-crafted queries                                                                          |
| 6             | Reprogramming ML system                         | Repurpose the ML system to perform an activity it was not programmed for                                                                         |
| 7             | Adversarial Example in Physical Domain          | Attacker brings adversarial examples into physical domain to subvertML system e.g: 3d printing special eyewear to fool facial recognition system |
| 8             | Malicious ML provider recovering training data  | Malicious ML provider can query the model used by customer and recover customer’s training data                                                  |
| 9             | Attacking the ML supply chain                   | Attacker compromises the ML models as it is being downloaded for use                                                                             |
| 10            | Backdoor ML                                     | Malicious ML provider backdoors algorithm to activate with a specific trigger                                                                    |
| 11            | Exploit Software Dependencies                   | Attacker uses traditional software exploits like buffer overflow to confuse/control ML systems                                                   |





