---
stand_alone: true
ipr: trust200902
docname: draft-sarikaya-6gip-aiml-security-privacy-00
cat: std
pi:
  toc: 'yes'
  tocompact: 'yes'
  tocdepth: '3'
  tocindent: 'yes'
  symrefs: 'yes'
  sortrefs: 'yes'
  comments: 'yes'
  inline: 'yes'
  compact: 'yes'
  subcompact: 'no'
title: 'Security and Privacy Implications of 3GPP AI/ML Networking Studies for 6G'
abbrev: AI/ML  security privacy implications
Date: 2024
author:
- ins: B. Sarikaya
  name: Behcet Sarikaya
  email: sarikaya@ieee.org
- ins: R. Schott
  name: Roland Schott
  org: Deutsche Telekom
  abbrev: Deutsche Telekom
  street: Deutsche-Telekom-Allee 9
  city: Darmstadt
  code: 64295
  country: Germany
  email: Roland.Schott@telekom.de
normative:
  RFC2904:
  RFC6749:
  RFC8446:

informative:
  TR22.874:
    title: Study on traffic characteristics and performance requirements for AI/ML model transfer in 5GS
    author: 
    - org: 3rd Generation Partnership Project
    date: 2021-12
  TR23.700-80:
    title: Study on 5G System Support for AI/ML-based Services
    author: 
    - org: 3rd Generation Partnership Project
    date: 2022-12
  TR23.700-82:
    title: Study on application layer support for AI/ML services
    author: 
    - org: 3rd Generation Partnership Project
    date: 2023-11
  IsNo20:
    target: https://www.ericsson.com/en/reports-and-papers/research-papers/secure-federated-learning-5g
    title: Secure Federated Learning in 5G Mobile Networks
    author:
    - ins: M. Isaksson
      name: Martin Isaksson
      org: ''
    - ins: C. Norrman
      name: Carl Norrman
      org: ''
    date: 2020-12
    series-info: 
      IEEE Global Communications Conference (GLOBECOMM 2020), December, 2020
  Srini21:
    target: https://www.kdnuggets.com/2021/11/difference-distributed-learning-federated-learning-algorithms.html
    title: Difference between distributed learning versus federated learning algorithms
    author:
    - ins: 
      name: Aishwarya Srinivasan
      org: ''
    date: 2021-11
    series-info: 
      KDnuggets Newsletter on Algorithms, Distributed Systems, Federated Learning November 19, 2021
  TR33.898: 
    title: Study on security and privacy of Artificial Intelligence/Machine Learning (AI/ML)-based services and applications in 5G
    author: 
    - org: 3rd Generation Partnership Project
    date: 2023-07
  TS33.501:
    target: https://www.3gpp.org/ftp/Specs/archive/33_series/33.501/33501-i30.zip
    title: Security Architecture and Procedures for 5G System
    Author:
    - org: 3rd Generation Partnership Project
    date: 2023-12
--- abstract


This document provides an overview of 3GPP work on Artificial Intelligence/ Machine Learning (AI/ML) networking. Application areas and corresponding proposed modifications to the architecture are identified. Security and privacy issues of these new applications need to be identified out of which IETF work could emerge.

--- middle

# Introduction

Artificial Intelligence (AI) has historically been defined as the science and engineering to build intelligent machines capable of carrying out tasks as humans do. Inspired from the way human brain works, machine learning (ML) is defined as the field of study that gives computers the ability to learn without being explicitly programmed. Since it is believed that the main computational elements a human brain are 86 billion neurons, the more popular ML approaches are using “neural network” as the model. Neural networks (NN) take their inspiration from the notion that a neuron’s computation involves a weighted sum of the input values. A computational neural network contains the neurons in the input layer which receive some values and propagate them to the neurons in the middle layer of the network, which is also called a “hidden layer”. The weighted sums from one or more hidden layers are ultimately propagated to the output layer, which presents the final outputs of the network. 

Neural networks having more than three layers, i.e., more than one hidden layer are called deep neural networks (DNN). In contrast to the conventional shallow-structured NN architectures, DNNs, also referred to as deep learning, made amazing breakthroughs since 2010s in many essential application areas because they can achieve human-level accuracy or even exceed human accuracy. Deep learning techniques use supervised and/or unsupervised strategies to automatically learn hierarchical representations in deep architectures for classification. With a large number of hidden layers, the superior performance of DNNs comes from its ability to extract high-level features from raw sensory data after using statistical learning over a large amount of data to obtain an effective representation of an input space. In recent years, thanks to the big data obtained from the real world, the rapidly increased computation capacity and continuously-evolved algorithms, DNNs have become the most popular ML models for many AI applications.


The performance of DNNs is gained at the cost of high computational complexity. Hence more efficient compute engines are often used, e.g. graphics processing units (GPU) and network processing units (NPU). Compared to the inference which only involves the feedforward process, the training often requires more computation and storage resources because it involves also the back propagation process.

Many DNN models have been developed over the past two decades. Each of these models has a different “network architecture” in terms of number of layers, layer types, layer shapes (i.e., filter size, number of channels and filters), and connections between layers. Three popular structures of DNNs: multilayer perceptrons (MLPs), convolution neural networks (CNNs), and recurrent neural networks (RNNs). Multilayer perceptrons (MLP) model is the most basic DNN, which is composed of a series of fully connected layers. In a fully connected layer, all outputs are connected to all inputs. Hence MLP requires a significant amount of storage and computation.

A convolution neural network (CNN) is composed of multiple convolutional layers. Applying various convolutional filters, CNN models can capture the high-level representation of the input data, making it popular for image classification and speech recognition tasks. Recurrent neural network (RNN) models are another type of DNNs, which use sequential data feeding. The input of RNN consists of the current input and the previous samples. RNN models have been widely used in the natural language processing task on mobile devices, e.g., language modeling, machine translation, question answering, word embedding, and document classification. 

AI/ML has very many applications, however, two areas have emerged that involve networking. One is the network optimization, time-series forecasting, predictive maintenance, Quality of Experience (QoE) modeling and the other is speech recognition, image recognition, video processing. In the former, the end device is the base station and the latter the UE {{TR22.874}}.

This document aims to present Artificial Intelligence Machine Learning (AIML) networking issues that may require further protocol work, mostly on the security and privacy aspects of networking.

# Training and Federated Learning

Training is a process in which a AI/ML model learns to perform its given tasks, more specifically, by optimizing the value of the weights in the DNN. A DNN is trained by inputting a training set, which are often correctly-labelled training samples. Taking image classification for instance, the training set includes correctly-classified images. The training process is repeated iteratively to continuously reduce the overall loss. Until the loss is below a predefined threshold, the DNN with high precision is obtained. After a DNN is trained, it can perform its task by computing the output of the network using the weights determined during the training process, which is referred to as inference. In the model inference process, the inputs from the real world are passed through the DNN. Then the prediction for the task is output. For instance, the inputs can be pixels of an image, sampled amplitudes of an audio wave or the numerical representation of the state of some system or game. Correspondingly, the outputs of the network can be a probability that an image contains a particular object, etc.

With continuously improving capability of cameras and sensors on mobile devices, valuable training data, which are essential for AI/ML model training, are increasingly generated on the devices. For many AI/ML tasks, the fragmented data collected by mobile devices are essential for training a global model. In the traditional approaches, the training data gathered by mobile devices are centralized to the cloud datacenter for a centralized training. 

In Distributed Learning mode, each computing node trains its own DNN model locally with local data, which preserves private information locally. To obtain the global DNN model by sharing local training improvement, nodes in the network will communicate with each other to exchange the local model updates. In this mode, the global DNN model can be trained without the intervention of the cloud datacenter.

In 3GPP Federated Learning (FL) mode, the cloud server trains a global model by aggregating local models partially-trained by each end devices. The most agreeable Federated Learning algorithm so far is based on the iterative model averaging whereby within each training iteration, a UE performs the training based on the model downloaded from the AI server using the local training data. Then the UE reports the interim training results (e.g., gradients for the DNN) to the cloud server via 5G uplink (UL) channels. The server aggregates the gradients from the UEs, and updates the global model. Next, the updated global model is distributed to the UEs via 5G DL channels. Then the UEs can perform the training for the next iteration. 

Summarizing, we can say that distributed learning (DML) is about having centralized data but distributing the model training to different nodes, while federated learning (FL) is about having decentralized data and training and in effect having a central model {{Srini21}}



# Architecture

A new framework for protocols called Service based architecture (SBA) comprises Network Functions (NFs) that expose services through RESTful APIs has been defined. There are providers and consumers (publishers and subscribers) which are new functions in the system {{IsNo20}}.

3GPP core network has a new server function: The Network Data Analytics Function (NWDAF) provides analytics to 5GC Network Functions (NFs) and Operations and Management (OAM). An NWDAF may contain the Analytics logical function (AnLF): A logical function in NWDAF, which performs inference, derives analytics information and Model Training logical function (MTLF) which trains Machine Learning (ML) models and exposes new training services. The Application AI/ML operation logic is controlled by an Application Function (AF). Any AF request to the 5G System in the context of 5GS assistance to Application AI/ML operation should be authorized by the 5GC {{TR23.700-80}}.
 
NWDAF relies on various sources of data input including data from 5G core NFs, AFs, 5G core repositories, e.g., NRF, UDM, etc., and OAM data, including performance measurements (PMs), KPIs, configuration management data and alarms. An NWDAF may provide in turn analytics output results to 5G core NF, AFs, and OAM. Optionally, Data Collection Coordination Function (DCCF) and Messaging Framework Adaptor Function (MFAF) may be involved to distribute and collect repeated data towards or from various data sources. Note that AF contains a Network Exposure Function (NEF) if it is an untrusted AF. NEF may assist the AI/ML application server in scheduling available UE(s) to participate in the AI/ML operation (e.g. Federated Learning). Also, 5GC may assist the selection of UEs to serve as FL clients, by providing a list of target member UE(s), then subscribing to the NEF to be notified about the subset list of UE(s) (i.e. list of candidate UE(s)) that fulfill certain filtering criteria {{TR23.700-82}}.
  
# Security and Privacy

AI/ML networking raises many security and privacy issues. {{TR23.700-80}} and {{TR23.700-82}} identify a number of key issues (with the number currently being 7) and {{TR33.898}} presents a study on one of the key issues which will be detailed here.

{{TR23.700-80}} studies the exposure of different types of assistance information such as traffic rate, packet delay, packet loss rate, network condition changes, candidate FL members, geographical distribution information, etc. to AF for AI / ML operations. Some of assistance information could be user privacy sensitive, such as candidate FL members, geographical distribution information etc. There is a need to study how to protect such privacy-related assistance information. In addition, 5GC needs to determine which assistance information is required by AF to complete AI/ML operation and to avoid exposing information that is unnecessary for AI/ML operations.

Because of the use of Restful API which depend on the use of HTTP protocol, OAuth {{RFC6749}} protocol seems to be the natural choice here for authorization.

One solution can be developed reusing existing mechanism for authorization of 5GC assistance information exposure to AF. The solution is based on reusing the OAuth-based authorization mechanism 
OAuth {{RFC6749}} protocol extends traditional client-server authentication 
by providing a third party client with a token.  Since such
   token resembles a different set of credentials compared to those of
   the resource owner, the device needs not be allowed to use the
   resource owner's credentials to access protected resources. 

UE privacy profile/local policies stored in a database can also be employed to authorize UE-related 5GC assistance information exposure. UE privacy profile/local policies may also contain protection policies that indicate how 5GC assistance information should be protected (e.g., encryption, integrity protection, etc.). NWDAF via Network Exposure Function (NEF) sends the UE-related 5GC assistance information to AF when the local policies/UE privacy profile authorize the AF to access the information. According to the local policies/UE privacy profiles, NWDAF may need to protect the 5GC assistance information with security mechanisms.

Network Functions securely expose capabilities and events to 3rd party Application Functions (AF) via Network Exposure Function (NEF). The interface between the NEF and the Application Function needs integrity protection, replay protection, confidentiality protection for communication between the NEF and Application Function, and mutual authentication between the NEF and Application Function and protect internal 5G Core network information. The NEF also enable secure provision of information in the 3GPP network by authenticated and authorized AFs.

Security should be provided to support the protection of user privacy sensitive assistance information being exposed to AF. TLS 1.3 {{RFC8446}} is used to provide integrity protection, replay protection and confidentiality protection for the interface between the NEF and the AF {{TS33.501}}. 

# Work Points

Security and privacy of AI/ML Networking based services and applications need further work. {{TR33.898}} provides solutions to only one of many possible key issues. Each key issue has been in depth investigated in {{TR23.700-80}} and {{TR23.700-82}} from which new solutions can be developed.

We list below only some of the key issues identified: 

* enhance the mobile core network to expose information to the UE to facilitate its Application AI/ML operation (e.g. Model Training, Splitting and inference feedback etc.)

* expose UE-related information to an AF  ensuring that privacy and security requirements are met.

* additional parameters to be provisioned to the mobile core network by an external party for the assistance to Application AI/ML operation.

* Whether and how the existing the mobile core network data transfer/traffic routing mechanisms are re-used or enhanced to support the transmission of the Application AI/ML traffic(s) between AI/ML endpoints (i.e. UE and AF)?

* information to be provided by the mobile core network to the AF can help the AF to select and manage the group of UEs which will be part of FL operation.

* enhancing the architecture and related functions to support application layer AI/ML services

* supporting federated learning at application enablement layers

* enhancing the architecture and related functions to support management and/or configuration for split AI/ML operation, and in-time transfer of AI/ML models. The management and configuration aspects including discovery of required nodes for split AI/ML operation and support of different models of AI/ML operation splitting.

# Security Considerations

Security considerations of AI/ML Networking is TBD.



# IANA Considerations

There are no IANA considerations for this document.


# Acknowledgements

We acknowledge useful comments from Hesham ElBakoury.


--- back
