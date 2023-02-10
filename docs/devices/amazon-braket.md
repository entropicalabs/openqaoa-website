# Amazon Braket

OpenQAOA supports Braket and all gate-based QPU present on braket. For the freshest list, please check the [Braket's documentation](https://docs.aws.amazon.com/braket/latest/developerguide/braket-devices.html)

Currently, the available devices are

| Item         | Device Name  | Paradigm	 |  Type    | Device ARN   | Region |
|--------------|--------------|--------------|----------|--------------|--------|
| IonQ                   |  ionQdevice |gate-based | QPU | arn:aws:braket:::device/qpu/ionq/ionQdevice           | us-east-1|
| Oxford Quantum Circuits|  Lucy       |gate-based | QPU | arn:aws:braket:eu-west-2::device/qpu/oqc/Lucy         | eu-west-2|
| Rigetti                |  Aspen M-3  |gate-based | QPU | arn:aws:braket:us-west-1::device/qpu/rigetti/Aspen-M-3| us-west-1|
| AWS                    |  SV1        |gate-based | SIM | arn:aws:braket:::device/quantum-simulator/amazon/sv1  | *        |
| AWS                    |  DM1        |gate-based | SIM | arn:aws:braket:::device/quantum-simulator/amazon/dm1  | *        |
| AWS                    |  TN1        |gate-based | SIM | arn:aws:braket:::device/quantum-simulator/amazon/tn1  | *        |

!!! danger
    **Any computations on AWS braket carry a financial cost**. Please, make sure you are conformable with the latest [Amazon Braket pricing model](https://docs.aws.amazon.com/braket/latest/developerguide/braket-pricing.html) before continuing! 

## How to connect to AWS braket

There are two ways to access the devices on Braket

### Access through your own laptop

In order to access Braket's services from your own device you will need to authenticate. This is a two step process. First, you need to create a pair of Access Key and Secret Access Key

!!! danger
    Access keys are **really** dangerous. If leaked, they may compromise your AWS account. 

To create and configure the access keys follow the [this guide](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html)

If you are on a unix machine, then you will be able to set your credentials through the AWS Command Line Interface

```Bash
$ aws configure
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]: us-west-2
Default output format [None]: json
```

OpenQAOA, via the Braket SDK, checks for the credentials on your local machine, and proceeds to authenticate.

### Access through Braket hosted notebooks

You can get started by following [Braket's official docs](https://docs.aws.amazon.com/braket/latest/developerguide/braket-get-started.html). Once you are on a hosted notebook, all you will need to do is to create an `aws device`. 


#### Creating a Braket device

The procedure is simple:

```Python
q = QAOA()

sv1_device = create_device(location='aws', 
                            name='arn:aws:braket:::device/quantum-simulator/amazon/sv1', 
                            aws_region='us-west-1')
q.set_device(sv1_device)
```
