
## Lecture 1

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

## Lecture 2

We use `kivy` and `imslib`. They use OOP with inheritance to build a hierarchical program. Your widgets should subclass `BaseWidget`.

Here's a basic widget that maintains a frame counter on the top left of the screen:

```python
class MainWidget(BaseWidget):
	def __init__(self):
		super()
		self.info = topleft_label()
		self.add_widget(self.info)  # child widget update will get called
		self.frames = 0

		self.last_keycode = None
		self.last_modifiers = None

	# called every frame
	def on_update(self):
		self.frames += 1
		self.info.text = f"{self.frames}\n"
		self.info.text += f"{self.last_keycode} | {self.last_modifiers}"

	# allows us to handle input
	def on_key_down(self, keycode, modifiers):
		self.last_keycode = keycode
		self.last_modifiers = modifiers
		
```

We are also displaying the last input. There is also some utility that displays the currently pressed keys on the screen for debugging purposes.

Now, let's see how to handle generating audio.

```python
class SineGenerator():
	def __init__(self):
		super()
		self.frame = 0
		self.freq = 440
		self.gain = 0.1

	def generate(self, num_frames, num_channels):
		n = np.arange(self.frame, self.frame + num_frames)
		theta = 2.0 * np.pi * self.freq * n / Audio.sample_rate
		output = self.gain * np.sin(theta)
		self.frame += num_frames

		# True means there is more audio
		# you could imagine a sound effect that is finite
		return (output, True)

	def set_freq(f):
		self.freq = f

	def set_gain(g):
		self.gain = g

audio = Audio(1)  # num channels = 1
sine_gen = SineGenerator()
audio.set_generator(sine_gen)  # audio will poll from the generator
```

Now you could do some work to have our `MainWidget` contain the `Audio` and `SineGenerator` instances, and then have `on_key_down` edit the frequency via `set_freq`.

You can also imagine a `Mixer` that handles a lot of `Generator`s and sums their audio together. You will have to handle the `is_done` flag returned by each generator.

There is also an `AudioWriter` utility that can be used for debugging. The main `Audio` class can adda  listener function which writes to the `AudioWriter`.