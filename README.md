# tf_keras_generator_with_targets
## Simple Custom TensorFlow image Generator where **target** values are also **modified**

When having a dataset of images with coordinates as targets (e.g., vertices of a shape, coordinates for body landmarks), we want to augment the dataset, but we need a way to implement the ImageDataGenerator to modify the target variables according to image transformation. Ffor example, if the image is flipped horizontally, the target coordinates must be flipped also.

---
## Usage in Mix on Pix
I encountered this issue when building the model to determine the vertices (angular points) of shapes like triangles or rectangles for the Auto-Shapes functionnality of the iOS app **[Mix on Pix](https://apps.apple.com/us/app/mix-on-pix-text-on-photos/id633281586)**.

The Image generator is very important as it allows to augment the image dataset by doing rotations (0-360), horizontal and vertical flip of the images.

---

### Generator
- Simple generator: [tf_generator_with_target.ipynb](tf_generator_with_target.ipynb). Supports horizontal and vertical flip.

---
When used in the code to fit the model, we can now do:

```
batch_size = 64
pixto_gen = MyDataGenerator(partition, X_train, Y_train, batch_size=batch_size, n_vertices=3)
```

and then:
```
#Fit the model with data Augmentation
epochs = 36 
history = model.fit(x=pixto_gen,
                    epochs = epochs, validation_data = (X_val,Y_val),
                    verbose = 1, steps_per_epoch=X_train.shape[0] // batch_size
                    , callbacks=[learning_rate_reduction])
```                              

Note that this syntax where we directly specify `model.fit(x=pixto_gen` is new to TensorFlow 2.1.
Previous versions must use `model.fit_generator`. 