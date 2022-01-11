# Hybrid-Images

According to Wikipedia, a hybrid image is an image perceived in one of two different ways, depending on viewing distance, based on how humans process visual input.

Here are some examples:

<div  style="display: flex; align-items: flex-start; flex-direction: column; justify-content:space-evenly; width=100%;">
<p align='center' style="display: flex; align-items: center; justify-content: space-evenly; margin:15px; width:100%;">
    <img src='./images/example-1.jpg' width='55%'>
    <img src='./images/example-1.jpg', width='15%'>
</p>
<p align='center' style="display: flex; align-items: center; justify-content: space-evenly; margin:15px; width:100%;">
    <img src='./images/example-2.jpg' width='55%'>
    <img src='./images/example-2.jpg', width='15%'>
</p>
<p align='center' style="display: flex; align-items: center; justify-content: space-evenly; margin:15px; width:100%;">
    <img src='./images/example-3.jfif' width='55%'>
    <img src='./images/example-3.jfif', width='15%'>
</p>
</div>
<br />
<br/>
As we can observe, the images look different at different distances. In the bigger image, we pay more attention to the details and high frequencies, and in the smaller image, we pay attention to low-frequency elements like colors.

So when we take low frequencies from one image, and we take high frequencies from the other one and combine them, we obtain a picture like that.
<br />
To work this out, we must transfer images to the frequency domain. The example we will work on is composed of these two images.
<br />

<div style="display: flex; align-items: flex-start; flex-direction: row; justify-content:space-evenly; width=100%;">
    <img src='./images/Harry.jpg' style="width:40%">
    <img src='./images/Voldemort.jpg' style="width:40%">
</div>

<br />
At first, we need to align these images. To do that, we use a **Transition+Scale** transformation. A good aligning point would be the eyes. So at the beginning of the program, it will ask you to click on the subject eyes in each image. After that, it will automatically generate the transformed results for you.
<br />
<br/>
Here are the transformed images:
<br />
<br/>
<div style="display: flex; align-items: flex-start; flex-direction: row; justify-content:space-evenly; width=100%;">
    <img src='./images/Harry-transformed.jpg' style="width:40%">
    <img src='./images/Voldemort-transformed.jpg' style="width:40%">
</div>
<br />
Now that we aligned images adequately, we get to the main section. We must convert these images to their frequency domain using **Discrete Fourier Transform**. One way to achieve the same result faster is using the **Fast Fourier Transform**. I used NumPy _ff2_ function to transfer images to the frequency domain, and I used fftshift to transfer the low frequencies to the center of the images. 
<br/>
<br/>
Here are the frequency domains of Harry and Voldemort!
<br/>

<div style="display: flex; align-items: flex-start; flex-direction: row; justify-content:space-evenly; width=100%;">
    <div style="display: flex; align-items: center; flex-direction: column; justify-content:space-evenly; width:40%;">
        <h2 style="padding:20px; "> Harry </h2>
        <img src='./images/Harry-dft.jpg' style="width:100%">
    </div>
    <div style="display: flex; align-items: center; flex-direction: column; justify-content:space-evenly; width:40%;">
        <h2 style="padding:20px; "> Voldemort </h2>
        <img src='./images/Voldemort-dft.jpg' style="width:100%;">
    </div>
</div>

The next step is to separate high frequencies from **Harry** and low frequencies from **Voldemort**. We use a Gaussian Filter to achieve a softer final result.
We need a Gaussian Low Pass Filter and a Gaussian High Pass Filter. If we present the standard derivation of low pass with $s$ and high pass with $r$, for this example, we used:
$$ s = 25 $$
$$ r = 35 $$

Low Pass:

$$
H(u, v) = e ^ {-\frac{D^2(u, v)}{2s^2}}
$$

High Pass:

$$
H(u, v) = 1 - e ^ {-\frac{D^2(u, v)}{2r^2}}
$$

Here are the results for Gaussian High Pass and Gaussian Low Pass mask:
<br/>
<br/>

<<p align='center' style="display: flex; align-items: flex-start; flex-direction: row; justify-content:space-evenly; width=100%;">
<img src='./images/Harry-highpass.jpg' style="width:40%">
<img src='./images/Voldemort-lowpass.jpg' style="width:40%;">

</p>

If we apply these masks to them we acheive these results:
<br/>

<p align='center' style="display: flex; align-items: flex-start; flex-direction: row; justify-content:space-evenly; width=100%;">
<img src='./images/Harry-highpassed.jpg' style="width:40%">
<img src='./images/Voldemort-lowpassed.jpg' style="width:40%;">
</p>

</p>

If we add them weighted, we use this equation for this example.
$$Hybird_f = 1.3 \times Harry_f + Voldemort_f$$

Here is the _Combined DFT_:

<div style="display: flex; align-items: flex-start; flex-direction: row; justify-content:space-evenly; width=100%;">
        <img src='./images/Combined-dft.jpg' style="width:60%">
</div>

If we transfer back this result to _Spatial Domain_, we should get a good result. I used numpy _ifft_ for this part.

Since every image has three channels called **RGB**, I split the channels, performed the same operations, and finally merged them back together.

Here is the final merged result:

<div style="display: flex; align-items: flex-start; flex-direction: row; justify-content:space-evenly; width=100%;">
        <img src='./images/Hybrid.jpg' style="width:60%">
</div>

If we look at it in different distances we will see a diiferent person in it.
<br/>

<p align='center' style="display: flex; align-items: center; justify-content: space-evenly; margin:15px; width:100%;">
    <img src='./images/Hybrid.jpg' width='55%'>
    <img src='./images/Hybrid.jpg', width='15%'>
</p>
