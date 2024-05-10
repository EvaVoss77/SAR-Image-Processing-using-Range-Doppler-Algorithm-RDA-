The algorithm used to carry out this processing is known as the Range-Doppler Algorithm (RDA). This algorithm implements a series of steps on the acquired data, both in the range domain and in the azimuth domain, as illustrated in the block diagram of Figure:

![diag](https://github.com/EvaVoss77/Basic-Processing-of-a-Primary-Radar/assets/126124561/eeda60bb-12f1-4fc0-89fc-2a994370fc57)

In this work, the Range Doppler Algorithm (RDA) is initially implemented using simulated Synthetic Aperture Radar (SAR) data focused on a point target. Subsequently, after completing the processing on the point target, the application of the algorithm is extended to real data sets acquired from an aircraft.

# **Range Compression**

To perform range compression, a filter adapted to each reception window was applied, with the form $h(t) = s*(-t)$, where $s(t)$ represents the transmitted signal. To improve the efficiency of the algorithm, the convolution theorem was implemented.

# **FFT in azimuth**

Because points located at the same distance $r_0$ have the same frequency behavior, when the azimuth data is transformed into the frequency domain, all these trajectories are concentrated in the same place. This allows only one trajectory to be straightened in the frequency domain.

# **Range Cell Migration Correction (RCMC)**

Once in the frequency domain, range migration correction is applied. This correction involves shifting each range vector depending on the frequency. Starting from 1 and knowing that t = $f λ r_0/(2v^2)$, it is deduced that the shift to be applied is given by:

$$ \vec{r_j} = \vec{r_0} \left(1 + \frac{1}{8}\left(\frac{\lambda^2 f^2}{v^2}\right)^2\right) $$

where $\vec{r_0}$ is the range vector corresponding to the window in which the radar and the surface were closest, and $\vec{r_j}$ is the range vector corresponding to each row. Since shifting may lead to the point at $r_1$ not being sampled, interpolation is necessary to correctly fit the data.

# **Compression and IFFT in Azimuth**

Once the correction is performed, azimuth compression can be carried out. Compression is done using a matched filter whose impulse response is given by:

$$h_{ac}(t) = s^*(-t) = \text{rect} \left(\frac{t}{2T_{ac}}\right) e^{-j\pi \frac{BW_{ac}}{T_{ac}} t^2}$$

where $T_{ac} = \frac{0.886\lambda r}{L_a v}$ is the azimuth chirp time, $BW_{ac} = \frac{1.772v}{L_a}$  is the azimuth chirp bandwidth, and $L_a$ is the physical length of the radar antenna.

To efficiently apply this filter, a transformation to the frequency domain is performed, and the convolution theorem is used on each column of the data matrix. That is, the multiplication of each point of the filter with each point of the column is carried out, followed by the inverse Fourier transform to return the output to the azimuth distance domain.

