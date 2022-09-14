---
layout: page
title: Automating Cell Annotation
description: Automated cell type annotation using a convolutional autoencoder.
img: assets/img/cell-type.PNG
importance: 2
category: research
---

<i>This work was presented and published in [2022 44th Annual International Conference of the IEEE Engineering in Medicine & Biology Society (EMBC)](https://ieeexplore.ieee.org/abstract/document/9871071).
</i>

<div class="row">
    <div class="col-sm mt-3" onClick="window.open('https://sindhurathiru.github.io/assets/pdf/ePoster_v3.pdf','_blank');">
        {% include figure.html path="assets/img/eposter.png" title="eposter" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    <i>(click image to open in new window)</i>
</div>

<h2>Convolutional Autoencoder Structure

{% highlight perldoc linenos %}

nFeatures = x_train.shape[1]
model = None
input_ae = Input(shape=(nFeatures, 1))

x = Conv1D(16, 3, activation="relu", padding="same")(input_ae)
x = Conv1D(16, 3, activation="relu", padding="same")(x)
x = MaxPooling1D()(x)
x = Conv1D(32, 3, activation="relu", padding="same")(x)
x = Conv1D(32, 3, activation="relu", padding="same")(x)
x = Dropout(0.7)(x)
encoded = MaxPooling1D()(x)

x = Conv1D(32, 3, activation="relu", padding="same")(encoded)
x = Conv1D(32, 3, activation="relu", padding="same")(x)
x = UpSampling1D()(x)
x = Conv1D(16, 3, activation="relu", padding="same")(x)
x = Conv1D(16, 3, activation="relu", padding="same")(x)
x = UpSampling1D()(x)
decoded = Conv1D(1, 3, activation='sigmoid', padding='same', name='ae_out')(x)

y = Flatten()(encoded)
y = Dense(64, activation = 'relu')(y)
y = Dense(24, activation = 'relu')(y)

output_class = Dense(4, activation = 'softmax', name = "class_out")(y)

{% endhighlight %}
