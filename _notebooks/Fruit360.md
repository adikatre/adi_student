```python
import tensorflow as tf
from tensorflow.keras.preprocessing.image import ImageDataGenerator
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Conv2D, MaxPooling2D, Flatten, Dense, Dropout
from tensorflow.keras.optimizers import Adam

# Define paths
train_dir = r'Documents\kaggle\fruits360\fruits-360_dataset\fruits-360\Training'
test_dir = r'Documents\kaggle\fruits360\fruits-360_dataset\fruits-360\Test'

# Image data generators for loading and augmenting the images
train_datagen = ImageDataGenerator(
    rescale=1./255,
    shear_range=0.2,
    zoom_range=0.2,
    horizontal_flip=True
)

test_datagen = ImageDataGenerator(rescale=1./255)

train_generator = train_datagen.flow_from_directory(
    train_dir,
    target_size=(100, 100),
    batch_size=32,
    class_mode='categorical'
)

test_generator = test_datagen.flow_from_directory(
    test_dir,
    target_size=(100, 100),
    batch_size=32,
    class_mode='categorical'
)

# Building the CNN model
model = Sequential([
    Conv2D(32, (3, 3), activation='relu', input_shape=(100, 100, 3)),
    MaxPooling2D(pool_size=(2, 2)),
    Conv2D(64, (3, 3), activation='relu'),
    MaxPooling2D(pool_size=(2, 2)),
    Conv2D(128, (3, 3), activation='relu'),
    MaxPooling2D(pool_size=(2, 2)),
    Flatten(),
    Dense(512, activation='relu'),
    Dropout(0.5),
    Dense(131, activation='softmax')  # 131 classes in the dataset
])

# Compile the model
model.compile(optimizer=Adam(), loss='categorical_crossentropy', metrics=['accuracy'])

# Determine steps per epoch and validation steps
steps_per_epoch = train_generator.samples // train_generator.batch_size
validation_steps = test_generator.samples // test_generator.batch_size

# Train the model with added print statements for debugging
try:
    history = model.fit(
        train_generator,
        steps_per_epoch=steps_per_epoch,
        epochs=15,
        validation_data=test_generator,
        validation_steps=validation_steps
    )
except Exception as e:
    print(f"An error occurred: {e}")

# Evaluate the model
loss, accuracy = model.evaluate(test_generator)
print(f'Test accuracy: {accuracy * 100:.2f}%')
```

    Found 67692 images belonging to 131 classes.
    Found 22688 images belonging to 131 classes.


    C:\Users\adity\AppData\Local\Programs\Python\Python312\Lib\site-packages\keras\src\layers\convolutional\base_conv.py:107: UserWarning: Do not pass an `input_shape`/`input_dim` argument to a layer. When using Sequential models, prefer using an `Input(shape)` object as the first layer in the model instead.
      super().__init__(activity_regularizer=activity_regularizer, **kwargs)


    Epoch 1/15


    C:\Users\adity\AppData\Local\Programs\Python\Python312\Lib\site-packages\keras\src\trainers\data_adapters\py_dataset_adapter.py:121: UserWarning: Your `PyDataset` class should call `super().__init__(**kwargs)` in its constructor. `**kwargs` can include `workers`, `use_multiprocessing`, `max_queue_size`. Do not pass these arguments to `fit()`, as they will be ignored.
      self._warn_if_super_not_called()


    [1m2115/2115[0m [32m━━━━━━━━━━━━━━━━━━━━[0m[37m[0m [1m720s[0m 338ms/step - accuracy: 0.4970 - loss: 1.9714 - val_accuracy: 0.9128 - val_loss: 0.3601
    Epoch 2/15
    [1m   1/2115[0m [37m━━━━━━━━━━━━━━━━━━━━[0m [1m6:14[0m 177ms/step - accuracy: 0.8750 - loss: 0.2425

    C:\Users\adity\AppData\Local\Programs\Python\Python312\Lib\contextlib.py:158: UserWarning: Your input ran out of data; interrupting training. Make sure that your dataset or generator can generate at least `steps_per_epoch * epochs` batches. You may need to use the `.repeat()` function when building your dataset.
      self.gen.throw(value)


    An error occurred: 'NoneType' object has no attribute 'items'
    [1m709/709[0m [32m━━━━━━━━━━━━━━━━━━━━[0m[37m[0m [1m46s[0m 65ms/step - accuracy: 0.9185 - loss: 0.3224
    Test accuracy: 91.70%

