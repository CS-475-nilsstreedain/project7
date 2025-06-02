# project7
## Note:
- This is a CPU Cluster project. The flip machines are not a cluster, so MPI won't run there.
- You will need to use the submit-* machines.
- Check your MPI_Send and MPI_Recv calls carefully. The number of items you receive should match the number of items you send.

## Introduction
I like Wikipedia's definition of Fourier Transform:
> "In mathematics, the Fourier transform (FT) is an integral transform that takes a function as input and outputs another function that describes the extent to which various frequencies are present in the original function."

In English, what does this actually mean? It means you take an apparently noisy signal, i.e., an array of length NUMELEMENTS random-looking values sampled over time, like this:
![image](https://github.com/user-attachments/assets/84c23940-8185-476d-a938-4656ab3065b2)

You then multiply each array element by a sine wave of a certain frequency.

You then do this again for a sine wave of a different frequency:

Now, if the signal is truly random, there will be positive and negative array values in there being multiplied by positive and negative sine values, giving both positive and negative products. So, by doing this, you get Sum[*] array values that are close to zero.

But that is if the signal is truly random. What if it isn't? What if it has one or more secret sine waves embedded in the noise? Then, when you multiply by the sine wave of that secret frequency, lots of positive numbers will get multiplied by positive numbers and lots of negative numbers will get multiplied by negative numbers, and that particular Sums[ ] value will be much larger than zero. In fact, if you were to plot all the Sums[*], the Sums[] for the secret embedded frequencies will really stand out.

I know what you're thinking. You are worried that it will be so subtle that you won't notice it. Not a problem. It's not subtle. It will reach out and slap you in the face.

What if there are two secret sine waves? In that case you will see two big humps in the Sums[ ] plot.

Scientists and engineers use the Fourier analysis to see if there are regular patterns in a thought-to-be-random signal. For example, you might be checking for:
- 60-cycle hum contamination in a sensor signal, or
- intelligent life elsewhere in the universe that have their own radio signals,
- etc.

The problem is that these signals can be quite large. Your job in this project is to implement this using MPI, and compare the performances across different numbers of processors.

## Requirements:
1. Read one of the signal files, either in text format bigsignal.txt or in binary format bigsignal.bin. Get one or both by right-clicking on it. Each file has 1,048,576 (=1*1024*1024 =1M) signal ampliudes in it. You can look at the text version if you want. You don't see any regular patterns there, right?
2. To be sure that you downloaded the complete file, check the file size against:
    - bigsignal.bin	4194304
    - bigsignal.txt	9437184

To read the signal data, see the code below.

3. A not-paralleled C/C++ way of doing the multiplying and summing would be:
```
BigSums[0] = 0.;
for( int p = 1; p < MAXPERIODS; p++ )
{
	BigSums[p] = 0.;
	float omega = F_2_PI / (float)p;
	for( int t = 0; t < NUMELEMENTS; t++ )
	{
		BigSums[p] += BigSignal[t] * sin( omega*(float)t );
	}
}
```

4. Do this using MPI parallelism.
Each processor will be responsible for Fourier'ing (NUMELEMENTS/NumCpus) elements.

5. Scatterplot the MAXPERIODS Fourier Sums[*] vs. the period.

There are two secret sine-waves in the signal. Scatterplotting Sums[*] will reveal them. You will see two spikes. The period on the X-axis below each spike tells you what the secret periods are. Report these in your PDF report.

Don't worry -- if you have done everything correctly, it will be really, really obvious.

6. In your PDF report, tell us what you think the two secret sine waves' periods are.
7. Draw a graph showing the performance versus the number of MPI processors you use. Pick appropriate units. Make "faster" go up.
8. Turn into Canvas:
    1. Your source code file (.cpp).
    2. Your commentary in a PDF file.
9. Your commentary PDF should include:
    1. Show the Sums[*] vs. period scatterplot.
    2. State what the two secret sine-wave periods are.
    3. Show your graph of Performance vs. Number of Processors used.
    4. What patterns are you seeing in the performance graph?
    5. Why do you think the performances work this way?

## Skeleton Code:
The skeleton code is in the file proj07.cpp.

Left-click to see it.

Right-click to download it.

## Grading:
Feature | Points
-|-
Fourier Sums[*] vs. period graph | 30
Report the two correct sine-wave periods | 30
Performance graph | 30
Commentary in the PDF file | 30
Potential Total | 120
