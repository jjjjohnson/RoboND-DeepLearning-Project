Deep Learning Project
---------------------

Data collection:
----------------

-   Data gathered by following the hero in a very dense crowd. In this case, the quad is in the follow mode and the hero’s back images are collected. The dense
cloud also provides enough training data for distinguishing people and hero.

-   Data gathered by capturing images of hero from far away. In order to improve
    the accuracy of hero classification from far away.

-   Data gathered while in patrol directly over the hero, while the hero zigzag.
    In order to get the 360 view of hero walking.

Segmentation Network 
=====================

-   Encoder block:

```

    encoder1 = encoder_block(inputs, filters=32, strides=2)

    encoder2 = encoder_block(encoder1, filters=64, strides=2)

    encoder3 = encoder_block(encoder2, filters=128, strides=2)

```

-   1x1 Convolution

```
 conv_1x1 = conv2d_batchnorm(encoder3, filters=264, kernel_size=1, strides=1)
```

-   Deconder block:

```

    decoder1 = decoder_block(small_ip_layer=conv_1x1, large_ip_layer=encoder2, filters=128)

    decoder2 = decoder_block(small_ip_layer=decoder1, large_ip_layer=encoder1, filters=64)

    decoder3 = decoder_block(small_ip_layer=decoder2, large_ip_layer=inputs, filters=32)
```

-   Hyper-parameters


	learning_rate = 0.05
	
	batch_size = 128
	
	num_epochs = 7
	
	steps_per_epoch = 500
	
	validation_steps = 50


-   Training result:
-   
	Training loss: 0.0140
	validation loss: 0.0391


![](https://i.imgur.com/beFw7GI.png)

-   Images while following the target
![](https://i.imgur.com/1JCqDDU.png)


-   Images while at patrol without target
![](https://i.imgur.com/BDVLMlj.png)

- Final grade score: **0.456**
