# Chapter 17: Building Sound Effects

In Chapter 16, we learned how a list of samples can describe a tone, noise, or silence.

Now we ask:

> How can several simple sounds become one interesting effect?

A sound effect can change over time.

It can fade in.

It can fade out.

It can place one sound after another.

It can combine sounds that happen at the same time.

These are list operations. The samples are the material; the operations shape what we hear.

~~~text
A waveform supplies sound.
An envelope shapes it.
Sequencing and overlap combine it.
~~~

---

## Part 17.1 - An Envelope Is a List of Controls

An envelope is a sequence of values that controls another sequence.

~~~python
envelope = [0.0, 0.1, 0.2, 0.3, 0.4]
~~~

| Question | Answer |
| :--- | :--- |
| True or false: every value in envelope is between zero and one. | **True.** |
| What could the first value mean? | Let no sound through at the beginning. |
| What could the last value mean? | Keep 40 percent of the original sample at that position. |
| What kind of pattern do the values make? | A gradual rise. |
| What does the envelope describe? | How strongly the sound is allowed through over time. |

### Fundamental Questions About Envelopes

| Question | Answer |
| :--- | :--- |
| What is an envelope? | A changing control signal applied to another waveform. |
| Does an envelope need to be audible by itself? | No. It shapes another sound. |
| What does a value of zero do when multiplied by a sample? | It makes that output sample zero. |
| What does a value of one do? | It leaves the sample unchanged. |
| What is a fade? | An envelope that gradually changes the sound's level. |

---

## Part 17.2 - Multiplication Makes a Fade

To fade a sound, multiply each sample by its matching envelope value.

~~~python
waveform = [1.0, 0.5, 0.0, -0.5, -1.0]
envelope = [0.0, 0.25, 0.5, 0.75, 1.0]

faded = [
    sample * level
    for sample, level in zip(waveform, envelope)
]
~~~

| Question | Answer |
| :--- | :--- |
| True or false: zip pairs samples with envelope values by position. | **True.** |
| What is the first output value? | 0.0. The first sample is multiplied by zero. |
| What is the last output value? | -1.0. It is multiplied by one. |
| What kind of fade is this? | A fade in. |
| Why must the lists have corresponding positions? | Each envelope value controls the sample at the same time. |

### Fundamental Questions About Element-Wise Multiplication

| Question | Answer |
| :--- | :--- |
| What does element-wise multiplication do? | Multiplies values that occupy matching positions in two lists. |
| What does the waveform contribute? | The tone's changing positive and negative shape. |
| What does the envelope contribute? | The changing level over time. |
| What does their product become? | A waveform whose level follows the envelope. |
| What happens if the envelope list is shorter? | zip stops at the shorter list, so the result is shorter. |

> ### Law of Element-Wise Multiplication
> An envelope shapes a waveform by multiplying matching positions together — the waveform supplies the shape, the envelope supplies the changing level, and their product carries both.

---

## Part 17.3 - Build a Fade from Positions

The envelope can be generated from the sample positions.

~~~python
sample_count = 10
fade_in = [n / sample_count for n in range(sample_count)]
~~~

| Question | Answer |
| :--- | :--- |
| True or false: fade_in begins at zero. | **True.** |
| What is its last value? | 0.9. |
| Why does it not reach 1.0? | The last index is 9, and 9 divided by 10 is 0.9. |
| What would make the values reach one at the end? | Divide by sample_count minus one, when sample_count is greater than one. |
| What does this reveal? | The fade is a generated pattern based on position. |

A fade out reverses the direction.

~~~python
sample_count = 10
fade_out = [
    1 - n / (sample_count - 1)
    for n in range(sample_count)
]
~~~

| Question | Answer |
| :--- | :--- |
| What is the first value of fade_out? | 1.0. |
| What is the last value? | 0.0. |
| What does the formula change from the fade in? | The level decreases as the position increases. |
| What edge case should we consider? | A one-sample envelope would divide by zero. |
| What could a function do with that edge case? | Return a defined value or require at least two samples. |

### Fundamental Questions About Position-Based Envelopes

| Question | Answer |
| :--- | :--- |
| What does the sample index provide? | Position in time. |
| What does the envelope formula provide? | A level at that position. |
| How can a fade in and fade out differ? | Their levels move in opposite directions. |
| Can an envelope have a more complex shape? | Yes. It can rise, hold, fall, or repeat smaller changes. |

---

## Part 17.4 - Apply an Envelope to a Tone

Let us make a reusable function.

~~~python
def apply_envelope(waveform, envelope):
    return [
        sample * level
        for sample, level in zip(waveform, envelope)
    ]
~~~

| Question | Answer |
| :--- | :--- |
| True or false: apply_envelope changes the original waveform list. | **False.** It returns a new list. |
| What does waveform provide? | The original sound. |
| What does envelope provide? | One level for each corresponding sample. |
| What happens to a negative sample during a fade? | It remains negative, but its magnitude changes. |
| What should normally be true of the two list lengths? | They should match, so the envelope covers the full sound. |

~~~python
sample_rate = 100
tone = cosine_wave(10, sample_rate, 1)

fade_in = [
    n / (len(tone) - 1)
    for n in range(len(tone))
]

faded_tone = apply_envelope(tone, fade_in)
~~~

| Question | Answer |
| :--- | :--- |
| How long is tone? | One second. |
| How many samples does it contain? | 100. |
| What happens to the beginning of faded_tone? | Its level begins near zero. |
| What happens to the end? | It approaches the original tone's level. |
| What would a plot show? | The cosine still oscillates, while its outer height grows. |

### Visual Check: The Wave Inside the Envelope

Plot the original tone and faded_tone.

| Question | Answer |
| :--- | :--- |
| Does the fade erase the wave's cycles? | No. It changes their height. |
| What shape does the outer boundary of the samples suggest? | The envelope. |
| What remains visible inside that boundary? | The waveform. |
| What does this image of the plot teach? | A sound can contain a fast wave and a slower control pattern at once. |

---

## Part 17.5 - Sequencing Places Sounds One After Another

Digital audio is a list, so one sound can follow another by joining lists.

~~~python
first = [1, 1, 1]
second = [2, 2]
combined = first + second
~~~

| Question | Answer |
| :--- | :--- |
| True or false: combined contains five values. | **True.** |
| What are the first three values? | The values from first. |
| What are the final two values? | The values from second. |
| What does list addition mean here? | Concatenation: place one sequence after the other. |
| Does concatenation make the sounds overlap? | No. The second begins after the first ends. |

Now use waveforms and silence.

~~~python
sample_rate = 22050
high_tone = cosine_wave(880, sample_rate, 0.5)
gap = silence(int(sample_rate * 0.25))
low_tone = cosine_wave(440, sample_rate, 0.5)

sequence = high_tone + gap + low_tone
~~~

| Question | Answer |
| :--- | :--- |
| How long is high_tone? | Half a second. |
| How long is gap? | A quarter of a second. |
| How long is low_tone? | Half a second. |
| How long is sequence? | 1.25 seconds. |
| What might the listener hear? | A high tone, a pause, then a lower tone. |

### Fundamental Questions About Sequencing

| Question | Answer |
| :--- | :--- |
| What does sequencing do? | Places sounds in time one after another. |
| What operation joins sample lists? | List concatenation with plus. |
| What does silence contribute? | A timed gap. |
| Does sequencing change the samples inside each segment? | No. It changes where the segments occur in the larger list. |

> ### Law of Sequencing
> Sequencing sounds in time is list concatenation; overlapping sounds in time is element-wise addition — the two operations answer “when” and “together,” and should not be confused.

| What can a loop add? | Repeated motifs. |

---

## Part 17.6 - A Loop Can Repeat a Motif

A motif is a small recognizable pattern.

~~~python
motif = cosine_wave(440, 22050, 0.2) + silence(2205)

song = []
for _ in range(3):
    song += motif
~~~

| Question | Answer |
| :--- | :--- |
| True or false: the loop adds the motif three times. | **True.** |
| How long is the tone portion? | 0.2 seconds. |
| How long is the silence? | 0.1 seconds, because 2205 samples at 22,050 per second last 0.1 seconds. |
| How long is one motif? | 0.3 seconds. |
| How long is song? | 0.9 seconds. |
| What did the loop provide? | Repetition of a timed sound pattern. |

### Fundamental Questions About Motifs

| Question | Answer |
| :--- | :--- |
| What is a motif? | A short repeated musical or sound pattern. |
| What does the motif list contain? | Sound segments in their intended order. |
| What does the loop control? | How many times the pattern repeats. |
| What could change the motif while preserving its structure? | Change the tone, duration, or silence segment. |
| What does this have in common with procedural image patterns? | A small rule or pattern can be repeated to create a larger artifact. |

---

## Part 17.7 - Overlap Adds Sounds Together

Sequencing places sounds one after another.

Overlap places sounds at the same time.

For equal-length sample lists, add corresponding values.

~~~python
first = [0.2, 0.0, -0.2]
second = [0.1, 0.1, 0.1]

mixed = [
    a + b
    for a, b in zip(first, second)
]
~~~

| Question | Answer |
| :--- | :--- |
| True or false: mixed contains three samples. | **True.** |
| What is the first mixed value? | 0.3. |
| What does each output position combine? | The samples that occur at the same time in both sounds. |
| What does overlap do in audio? | It combines simultaneous waveforms. |
| What can overlapping tones create? | Chords, harmonies, or a layered sound effect. |

### Fundamental Questions About Overlap

| Question | Answer |
| :--- | :--- |
| What does sequencing combine? | Different times. |
| What does overlap combine? | The same times. |
| What Python operation models overlap? | Element-wise addition. |
| What should be true of the lists? | They should describe the same time span, or be padded deliberately. |
| What can happen to amplitude when sounds are added? | The sum can exceed the allowed range. |

---

## Part 17.8 - Keep the Mix in Range

Suppose several sounds are added.

~~~python
tone_a = cosine_wave(220, 22050, 0.5)
tone_b = cosine_wave(440, 22050, 0.5)
tone_c = cosine_wave(880, 22050, 0.5)

mixed = [
    (a + b + c) / 3
    for a, b, c in zip(tone_a, tone_b, tone_c)
]
~~~

| Question | Answer |
| :--- | :--- |
| True or false: each output sample combines three simultaneous samples. | **True.** |
| Why divide by three? | To reduce the level of the sum and help keep it within range. |
| Does dividing by three guarantee the loudness will be perceptually balanced? | No. It is a simple way to control peak size. |
| What happens if a sample exceeds 1.0 or falls below -1.0? | It may clip when converted to the output format. |
| What should we inspect before writing the file? | The minimum and maximum sample values. |

A reusable mixer can make the operation explicit.

~~~python
def mix_two(first, second):
    count = min(len(first), len(second))
    return [
        (first[n] + second[n]) / 2
        for n in range(count)
    ]
~~~

| Question | Answer |
| :--- | :--- |
| What does mix_two return? | A new list containing the average of paired samples. |
| Why use the shorter length? | The function only combines positions present in both lists. |
| Is averaging the only possible mixing rule? | No. It is one simple level-control choice. |
| What should a more complete mixer decide? | How to handle different lengths and how to control output amplitude. |

### Fundamental Questions About Safe Mixing

| Question | Answer |
| :--- | :--- |
| What is clipping? | Cutting off waveform values that exceed the representable range. |
| Why can overlap cause clipping? | Several individually valid samples can sum to an invalid value. |
| What are two ways to reduce clipping? | Scale the sources down or scale the final mix. |
| Should we always divide by the number of sounds? | It is a useful starting rule, but the best level depends on the signals and desired effect. |
| What invariant should the final audio meet? | Every normalized sample should be within the chosen output range. |

> ### Law of Safe Mixing
> Adding several valid samples can produce an invalid sum — check and scale a mix before writing it, rather than assuming individually safe sources add up to a safe result.

---

## Part 17.9 - Sequence and Overlap Can Cooperate

A sound effect can use both operations.

~~~python
saw = sawtooth_wave(440, 22050, 0.25, amplitude=0.5)
gap = silence(int(22050 * 0.1))
tone = cosine_wave(440, 22050, 0.25, amplitude=0.5)

layered = [
    (a + b) / 2
    for a, b in zip(saw, tone)
]
effect = layered + gap + tone
~~~

| Question | Answer |
| :--- | :--- |
| What happens in layered? | The sawtooth and cosine overlap in time. |
| What happens after layered? | Silence is sequenced, then the cosine plays. |
| What operations are combined? | Element-wise addition and list concatenation. |
| What is the result? | One sample list representing a more structured effect. |
| What does composition contribute? | Small operations combine into a sound with timing and texture. |

### Fundamental Questions About Sound Composition

| Question | Answer |
| :--- | :--- |
| What does a waveform provide? | A source sound. |
| What does an envelope provide? | A changing level. |
| What does sequencing provide? | Order and spacing in time. |
| What does overlap provide? | Simultaneous layers. |
| What does a sound effect designer decide? | Which sources to use, how to shape them, and when they occur. |

---

## Part 17.10 - Reverb as Many Small Delays (Optional)

A room can make a sound persist after its source stops.

One simple way to imagine this is as many quieter, delayed copies.

~~~python
def add_delayed_echoes(sound, delays, gains):
    output_length = len(sound) + max(delays, default=0)
    output = [0.0 for _ in range(output_length)]

    for n, sample in enumerate(sound):
        output[n] += sample

    for delay, gain in zip(delays, gains):
        for n, sample in enumerate(sound):
            destination = n + delay
            output[destination] += sample * gain

    return output
~~~

| Question | Answer |
| :--- | :--- |
| True or false: each echo is a quieter copy of the original sound. | **True.** |
| What does delay control? | How many samples later the copy begins. |
| What does gain control? | How loud the copy is relative to the original. |
| Why does the function read from sound each time? | Each echo is based on the original input, not on an already echoed output. |
| What is one limit of this simple model? | It creates a few distinct echoes rather than the dense reflections of a real room. |

### Fundamental Questions About Echoes

| Question | Answer |
| :--- | :--- |
| What is an echo in sample-list terms? | A delayed, scaled copy of earlier samples. |
| What does a shorter delay sound like? | A closely spaced reflection. |
| What does a smaller gain sound like? | A quieter reflection. |
| How can many echoes suggest a room? | Their overlapping reflections create a lingering sound. |
| What idea does this preview? | Convolution can describe many scaled, delayed copies at once. |

---

## Part 17.11 - The Sound Pipeline

A complete effect can be described as a sequence of operations.

~~~text
choose waveform
      |
generate samples
      |
shape with an envelope
      |
sequence or overlap
      |
check amplitude range
      |
write and listen
~~~

| Question | Answer |
| :--- | :--- |
| What comes first? | Choose the sound sources and their parameters. |
| What does the envelope stage do? | Shapes how each sound changes over time. |
| What does sequencing decide? | When each sound occurs. |
| What does overlap decide? | Which sounds occur together. |
| Why check the range before writing? | To avoid clipping and distorted output. |
| What does listening add? | It tests the result as sound, not only as numbers or a plot. |

---

## Part 17.12 - Predict the Result

| Question | Answer |
| :--- | :--- |
| What is faded?<br><br>waveform = [1.0, 0.5]<br>envelope = [0.0, 1.0] | [0.0, 0.5]. |
| What is combined?<br><br>first = [0.1, -0.1]<br>second = [0.2, 0.3] | [0.3, 0.2]. |
| What is the length?<br><br>first = [1, 2, 3]<br>second = [4, 5]<br>result = first + second | Five values. Concatenation keeps both lists. |
| What is the duration of 11,025 samples at 22,050 Hz? | Half a second. |
| Does adding two samples that are each 0.7 stay within 1.0? | No. Their sum is 1.4. |
| What does multiplying a sample by zero do? | Makes that output sample silent. |
| What is a fade out? | An envelope whose level decreases toward zero. |
| What is the difference between sequence and overlap? | Sequence places sounds at different times; overlap combines samples at matching times. |

---

## Chapter Summary

| Question | Answer |
| :--- | :--- |
| What is an envelope? | A changing list of levels that shapes another waveform. |
| How does a fade change a sound? | It multiplies each sample by a corresponding level. |
| What does sequencing do? | Places sample lists one after another. |
| What does overlap do? | Adds corresponding samples so sounds occur together. |
| Why control levels when mixing? | To keep the final sample values inside the playable range. |
| What does a motif provide? | A short pattern that can be repeated. |
| What does a simple echo model do? | Adds delayed, quieter copies of a sound. |
| What is the central idea? | A sound effect can be built by shaping waveforms and arranging their samples in time. |

---

**==> A sound effect is a design of samples: shaped, placed, and combined. <==**
