# EloquentTinyML

This Arduino library is here to simplify the deployment of Tensorflow Lite
for Microcontrollers models to Arduino boards using the Arduino IDE.

Including all the required files for you, the library exposes an eloquent
interface to load a model and run inferences.

I just added another example (MnistExample, handwitten digits recognition).
In the folder MnistExampleJupyterNotebooks, you will find Jupyter notebooks that will allow you to train your Mnist model for different size of caracters

## Install

Clone this repo in you Arduino libraries folder.

```bash
git clone https://github.com/Roland75013/EloquentTinyML.git
```


## Use

```cpp
#include "Arduino.h"
#include <EloquentTinyML.h>
// select the size of the digits and the corresponding model. uncomment the corresponding lines.
//#include "8x8mnist.h"//
//#include "8x8sampledigit1.h"
//#include "mnist14x14.h"
//#include "mnist14x14-light.h"
//#include "14x14sampledigit96.h"
#include "mnist.h"
#include "sampledigit2.h"       // the handwitten digit to recognize


#define NUMBER_OF_INPUTS 28*28
#define NUMBER_OF_OUTPUTS 10
// in future projects you may need to tweek this value: it's a trial and error process
#define TENSOR_ARENA_SIZE 32*1024

Eloquent::TinyML::TfLite<NUMBER_OF_INPUTS, NUMBER_OF_OUTPUTS, TENSOR_ARENA_SIZE> ml((unsigned char*) mnist);


float output[NUMBER_OF_OUTPUTS];

void setup() {
    Serial.begin(115200);
}

long t;

void loop() {

    int i,maxi;
    float maxval;
    Serial.println("hello!!");
    
    t=millis();
    float predicted = ml.predict(input,output);
    Serial.println(millis()-t);

    maxval = -1.0e20;
    for (i=0;i < NUMBER_OF_OUTPUTS;i++) 
        {
        if (output[i] >= maxval) 
            {
            maxval = output[i];
            maxi = i;
            }
        Serial.printf("%f  ",output[i]);
        }
    Serial.println();
    Serial.printf("digit is: %d\n",maxi);
 
//    delay(1000);
}```
