## Deep Learning Project


### Data collection:
----------------

-   Data gathered by following the hero in a very dense crowd. In this case, the quad is in the follow mode and the hero’s back images are collected. The dense
cloud also provides enough training data for distinguishing people and hero.

-   Data gathered by capturing images of hero from far away. In order to improve the accuracy of hero classification from far away.

-   Data gathered while in patrol directly over the hero, while the hero zigzag. In order to get the 360 view of hero walking.

### Segmentation Network 
----------------

-   Encoder block: 

Endoder layer extract features from the image by shrinking the image wdith and height while adding more depth. This can achieve the goal of space invariance in the image. Three encoder layers are added with increasing numberof filters and half the dimension

```

    encoder1 = encoder_block(inputs, filters=32, strides=2)

    encoder2 = encoder_block(encoder1, filters=64, strides=2)

    encoder3 = encoder_block(encoder2, filters=128, strides=2)

```

-   1x1 Convolution:

1x1 convolution is used to preserve spacial information ant it often(not in this project) leads to dimension reductionality. In the project, I find the filters to be 264 yield the best performance.

```
 conv_1x1 = conv2d_batchnorm(encoder3, filters=264, kernel_size=1, strides=1)
```

-   Decoder block:

Decoder block helps in upsampling the previous layer to a higher resolution or dimension. The upsampling is achieved by the Bilinear Upsampler. The bilinear upsampling method does not contribute as a learnable layer like the transposed convolutions in the architecture and is prone to lose some finer details, but it helps speed up performance. Decoder block also includes skip connections from encoder layer, which are a great way to retain some of the finer details from the previous layers as we decode or upsample the layers to the original size. Since the decoder block doubles the dimension each layer, the number of decoder layer has to be the same as the encoder layer.

```

    decoder1 = decoder_block(small_ip_layer=conv_1x1, large_ip_layer=encoder2, filters=128)

    decoder2 = decoder_block(small_ip_layer=decoder1, large_ip_layer=encoder1, filters=64)

    decoder3 = decoder_block(small_ip_layer=decoder2, large_ip_layer=inputs, filters=32)
```

-   Hyper-parameters

Hyper-parameters are determined by trial and error. The guide line to choose them is keep the batch_size as big as possible and stops training(set num_epoch) when the val_loss does not improve.

```
	learning_rate = 0.05
	
	batch_size = 128
	
	num_epochs = 7
	
	steps_per_epoch = 500
	
	validation_steps = 50
```


### Training result:
----------------
The training loss is higher than the val_loss, which indicates a bit of overfitting.
```
	Training loss: 0.0140

	validation loss: 0.0391
```

![](https://i.imgur.com/beFw7GI.png)

-   Images while following the target
![](https://i.imgur.com/1JCqDDU.png)


-   Images while at patrol without target
![](https://i.imgur.com/BDVLMlj.png)

- Final grade score: **0.456**

### Further improvement
--------
In order to improve the classification accuracy, several work worth a shot in the future:

- Add drop out in the encoding and decoding layer to conbact overfitting.
- Replace encoding layer with pretrained model, such as VGG16.
- Try deeper encoding and decoding layer.
