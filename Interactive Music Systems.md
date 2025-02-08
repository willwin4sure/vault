
On the input side, the microphone transmits signals to an analog-to-digital converter (ADC) to interface with the computer. On the output side, the computer sends bits to a digital-to-analog converter (DAC) for the speaker to play.

Given some analog audio signal, we need to sample and quantize it in order to digitize it. Since human hearing caps out at around 20 kHz, some sampling theorem says it suffices to sample at around 40 kHz. However, in order to prevent [[aliasing]], you want to first remove the higher frequency sounds in the original signal.

The computer has an audio driver that needs to constantly feed data to the DAC. This is supported by a ring buffer data structure that the application writes into.

One fundamental tradeoff is the size of this buffer. A smaller buffer means lower latency, but also means you need to feed it more often.

Suppose we want to discretize a sine wave. We can do this via
$$
y[n] = A\sin \left( 2\pi f \cdot \frac{n}{F_{s}} \right), 
$$
where $f$ is the frequency in Hz of the wave and $F_{s}$ is the sampling frequency (e.g. $44100$ Hz).

Recall your Fourier series; any periodic signal can be constructed additively using sinusoids of various frequencies and amplitudes.