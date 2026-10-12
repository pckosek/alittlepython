# Chapter 16: Sound as a List of Samples

In Chapter 15, a rule answered a question about one pixel. Repeated across a rectangle, those answers became an image.

Now we ask:

> What if a sound is a list of values instead of a picture?

A computer does not store the moving air we hear. It stores numbers that describe the changing height of a sound wave. A speaker turns those numbers into motion.

We will begin with a list of numbers, then make the list repeat like a wave, then save it as a sound file.

~~~text
A pixel is a value at a place.
A sample is a value at a time.
~~~

---

## Part 16.1 - Sound Begins with Samples

A sample is one measurement of a sound wave at one instant.

~~~python
samples = [0.0, 0.5, 1.0, 0.5, 0.0, -0.5, -1.0, -0.5]
~~~

| Question | Answer |
| :--- | :--- |
| True or false: samples is a list of numbers. | **True.** |
| What does one number represent? | The amplitude of the sound wave at one instant. |
| What does a positive sample mean? | The wave is above its center line at that instant. |
| What does a negative sample mean? | The wave is below its center line. |
| Is this list already an ordinary audible tone? | It describes samples, but it is far too short to make a sustained tone. |

### Fundamental Questions About Samples

| Question | Answer |
| :--- | :--- |
| What is a sample? | One numerical value representing a wave at one moment. |
| What is digital audio? | A sequence of samples arranged in time. |
| Does a sample contain the entire sound? | No. The sound is represented by many samples in order. |
| Why does sample order matter? | It describes how the wave changes through time. |

---

## Part 16.2 - How Fast Are the Samples?

A sound needs a clock.

~~~python
sample_rate = 10
~~~

| Question | Answer |
| :--- | :--- |
| True or false: a sample rate of 10 means ten samples per second. | **True.** |
| How many samples occur in half a second at this rate? | 5. |
| How long do ten samples last at a rate of ten samples per second? | One second. |
| What is a common sample rate in our examples? | 44,100 samples per second. |
| Does sample rate describe pitch? | No. It describes how densely sound is measured in time. |

### Fundamental Questions About Sample Rate

| Question | Answer |
| :--- | :--- |
| What does a higher sample rate mean? | More samples are stored for each second. |
| How many samples are needed for a duration? | sample_rate multiplied by duration. |
| Why does sample rate matter? | It sets the time spacing between samples and affects how frequencies are represented. |
| What does sample rate not tell us by itself? | The frequency or shape of the sound. |

> ### Law of Sample Rate
> Sample rate is a clock, not a pitch — it sets how many values describe one second, independent of what those values contain.

---

## Part 16.3 - A List Can Describe a Ramp

Before a wave repeats, let us generate a simple list.

~~~python
values = [n / 10 for n in range(10)]
~~~

| Question | Answer |
| :--- | :--- |
| True or false: range(10) gives ten positions. | **True.** It gives 0 through 9. |
| What is the first value in values? | 0.0. |
| What is the last value? | 0.9. |
| Why does it stop at 0.9 rather than 1.0? | The last position is 9, and 9 divided by 10 is 0.9. |
| What shape would a plot make? | A line that rises from left to right. |
| What does list position represent in a sound plot? | The sample's order in time. |

### Fundamental Questions About a Ramp

| Question | Answer |
| :--- | :--- |
| What does n represent? | The position of one value in the generated list. |
| Is this ramp periodic? | No. It does not repeat a cycle. |
| Why begin with a ramp? | Its relationship between position and value is easy to see. |
| What must change for values to form a tone? | They must repeat in a periodic pattern. |

---

## Part 16.4 - A Cosine Repeats

A cosine wave is a repeating pattern.

~~~python
import math

waveform = [
    math.cos(2 * math.pi * n / 100)
    for n in range(100)
]
~~~

| Question | Answer |
| :--- | :--- |
| True or false: this list describes one complete cosine cycle. | **True.** Its phase advances through one turn across the sample positions. |
| What does math.cos return? | A value from -1 through 1. |
| What is the approximate value at sample zero? | 1, because cosine of zero is one. |
| Why multiply by 2 times pi? | One full cycle corresponds to 2 pi radians. |
| How many samples describe this cycle? | 100. |
| What is its amplitude? | 1. |

### Fundamental Questions About a Digital Cycle

| Question | Answer |
| :--- | :--- |
| What is a period? | The number of samples in one complete repeating cycle. |
| If 100 samples make one cycle at 100 samples per second, what is the frequency? | 1 Hz. |
| If the sample rate becomes 200 and the period remains 100, what is the frequency? | 2 Hz. |
| What is the relationship? | Frequency equals sample rate divided by samples per cycle. |

> ### Law of a Digital Cycle
> Frequency, sample rate, and samples-per-cycle are three views of the same relationship: `frequency = sample_rate / samples_per_cycle`.

---

## Part 16.5 - Frequency and Sample Rate Work Together

A digital cosine can be described using frequency and sample rate.

~~~python
def cosine_wave(frequency, sample_rate, duration, amplitude=1.0):
    count = int(sample_rate * duration)
    return [
        amplitude * math.cos(2 * math.pi * frequency * n / sample_rate)
        for n in range(count)
    ]
~~~

| Question | Answer |
| :--- | :--- |
| True or false: duration determines the number of samples. | **True.** The count is sample_rate times duration. |
| What does frequency control? | How many cycles occur per second. |
| What does sample rate control? | How many samples are used per second. |
| What does n divided by sample_rate represent? | The time of sample n, in seconds. |
| What does amplitude control? | The height of the wave values. |
| What is frequency measured in? | Hertz, or cycles per second. |

~~~text
sample index -> time -> phase -> cosine value -> amplitude-scaled sample
~~~

| Question | Answer |
| :--- | :--- |
| What is the sample index n? | The position of this sample in the list. |
| Why multiply time by frequency? | It counts how many cycles have elapsed by that time. |
| What does the cosine calculate? | The wave's value at the current phase. |
| What happens when frequency increases but sample rate stays fixed? | More cycles fit into each second. |

---

## Part 16.6 - Period, Frequency, and Duration

These are related questions, but they are not the same question.

~~~python
waveform = cosine_wave(
    frequency=5,
    sample_rate=500,
    duration=1
)
~~~

| Question | Answer |
| :--- | :--- |
| True or false: this sound lasts one second. | **True.** |
| How many samples are in the list? | 500. |
| How many cycles occur? | 5. |
| How many samples describe one cycle? | 500 divided by 5, or 100. |
| What happens if duration becomes two seconds? | The sample count and total cycles both double. |
| What happens if frequency becomes 10 while duration stays one second? | Ten cycles occur. |

### Fundamental Questions About Three Questions

| Question | Answer |
| :--- | :--- |
| What does duration ask? | How long does the sound continue? |
| What does frequency ask? | How quickly does the wave repeat? |
| What does sample rate ask? | How many values describe each second? |
| What is the sample count? | Sample rate multiplied by duration. |

---

## Part 16.7 - Amplitude Changes the Height

A wave can keep its pitch while changing its amplitude.

~~~python
quiet = cosine_wave(440, 44100, 1, amplitude=0.25)
loud = cosine_wave(440, 44100, 1, amplitude=0.75)
~~~

| Question | Answer |
| :--- | :--- |
| True or false: both lists have the same frequency. | **True.** Both use 440 Hz. |
| Do they have the same number of samples? | Yes. Their sample rate and duration match. |
| What differs? | The height of the sample values. |
| Which list has the larger peak amplitude? | loud, with amplitude 0.75. |
| Does changing amplitude change frequency? | No. |
| What is the sample range for amplitude 0.25? | From -0.25 through 0.25. |

### Fundamental Questions About Amplitude

| Question | Answer |
| :--- | :--- |
| What does amplitude describe? | The maximum distance of the wave from its center line. |
| What does larger amplitude usually sound like? | Louder, when played at the same output level. |
| What is the ordinary normalized range for audio samples? | -1.0 through 1.0. |
| What risk appears when values exceed that range? | They may clip or distort during conversion to the sound file. |

> ### Law of Amplitude
> Amplitude changes a wave's height, never its pitch — and normalized samples should stay within -1.0 to 1.0 to avoid clipping when written to a file.

---

## Part 16.8 - Writing a Sound File

A list of samples can be written as a WAV file.

~~~python
import struct
import wave

sample_rate = 44100
samples = cosine_wave(440, sample_rate, 1)

with wave.open("tone.wav", "wb") as audio_file:
    audio_file.setnchannels(1)
    audio_file.setsampwidth(2)
    audio_file.setframerate(sample_rate)

    for sample in samples:
        value = int(32767 * sample)
        audio_file.writeframesraw(struct.pack("<h", value))
~~~

| Question | Answer |
| :--- | :--- |
| True or false: this creates a one-second sound file. | **True.** It writes 44,100 samples at 44,100 samples per second. |
| What does one channel mean? | Mono audio: one sequence of samples. |
| What does setsampwidth(2) mean? | Each sample is stored using two bytes, or 16 bits. |
| Why multiply by 32767? | It maps normalized samples into the range of signed 16-bit integers. |
| What does struct.pack do? | It encodes one signed 16-bit integer in little-endian byte order. |
| What should the file sound like? | A steady tone near 440 Hz. |

---

## Part 16.9 - Other Wave Shapes

The cosine is useful, but we can make other wave shapes from the same values.

A square wave turns each cosine sample into one of two states.

~~~python
def square_wave(frequency, sample_rate, duration, amplitude=1.0):
    cosine = cosine_wave(frequency, sample_rate, duration)
    return [
        amplitude if value >= 0 else -amplitude
        for value in cosine
    ]
~~~

| Question | Answer |
| :--- | :--- |
| True or false: the output contains only two possible values. | **True.** Each sample becomes positive or negative amplitude. |
| What happens when the cosine value is zero or greater? | The square wave returns positive amplitude. |
| What happens when the cosine value is below zero? | It returns negative amplitude. |
| Does cycle timing come from the cosine? | Yes. The sign changes at the same cycle boundaries. |
| What changes compared with a cosine? | The shape of the wave. |

| Question | Answer |
| :--- | :--- |
| What does frequency mainly describe? | How quickly a pattern repeats. |
| What does waveform shape affect? | The tone's timbre, or quality. |
| Can a cosine and square wave share a pitch and still sound different? | Yes. They have different wave shapes. |
| Why compare them by listening? | Their different shapes produce different tone qualities. |

Noise uses random samples instead of one simple repeating cycle.

~~~python
import random

def noise_wave(sample_count, amplitude=1.0):
    return [
        random.uniform(-amplitude, amplitude)
        for _ in range(sample_count)
    ]
~~~

| Question | Answer |
| :--- | :--- |
| True or false: noise repeats the same sample pattern at regular intervals. | **False.** Each value is chosen independently. |
| What is its range when amplitude is 1.0? | From -1.0 to 1.0. |
| Does noise still have a sample rate? | Yes. The values still occupy positions in time. |
| Can noise be reproducible? | Yes, if the random generator is seeded. |
| What distinction should we keep? | Random values do not mean random timing or an unknown sample rate. |

A sawtooth is a repeating ramp. Silence is a list of zeroes.

~~~python
def sawtooth_wave(frequency, sample_rate, duration, amplitude=1.0):
    count = int(sample_rate * duration)
    return [
        amplitude * (2 * ((frequency * n / sample_rate) % 1) - 1)
        for n in range(count)
    ]

def silence(sample_count):
    return [0.0 for _ in range(sample_count)]
~~~

| Question | Answer |
| :--- | :--- |
| What does the sawtooth rule do with phase? | It repeats a ramp from low to high, then starts again. |
| What does multiplying by two and subtracting one do? | It maps phase from zero-to-one into minus-one-to-one. |
| What does silence(4) return? | Four zero-valued samples. |
| Is an empty list the same as silence? | No. An empty list has no duration. |
| Can cosine, square, sawtooth, and noise be used in sound effects? | Yes. They provide different starting materials. |

---

## Part 16.10 - Predict the Result

| Question | Answer |
| :--- | :--- |
| How many samples are created with sample_rate 1000 and duration 0.5? | 500. |
| How long are 2205 samples at 44,100 samples per second? | 0.05 seconds. |
| What is the frequency if 100 samples make one cycle at a sample rate of 1000? | 10 Hz. |
| What is the sample range for amplitude 0.4? | From -0.4 to 0.4. |
| What values can a square wave with amplitude 0.5 return? | -0.5 and 0.5. |
| What does silence(4) return? | Four zero-valued samples. |
| What changes when duration doubles but frequency and sample rate stay fixed? | The sample count and total cycles both double. |
| What changes when sample rate doubles but frequency and duration stay fixed? | The sample count doubles, and more samples describe each cycle. |

---

## Chapter Summary

| Question | Answer |
| :--- | :--- |
| What is digital sound? | A sequence of amplitude samples arranged in time. |
| What does sample rate describe? | The number of samples per second. |
| What does frequency describe? | The number of waveform cycles per second. |
| What does duration control? | How long the sequence lasts. |
| What does amplitude control? | The height of the waveform. |
| What is noise? | A sequence of varying samples without one simple repeating cycle. |
| What is silence? | A sequence of zero-valued samples with a duration. |
| What is the central idea? | Sound can be built sample by sample, with time, frequency, waveform, and amplitude expressed through lists and rules. |

---

**==> A sound is a pattern of values moving through time. <==**
