# Multiscale FWI

I implemented multiscale FWI as proposed by Bunks, et. al., 1995, to tackle the cycle-skipping problem. During the implementation,  
lowpass filtering is mandatory (doesn't has to be lowpass) to make it simulated in some range of frequencies. Started from stage 1  
where the cut off frequency is 5 Hz, then we go to 10 Hz, and lastly we involved nearly all range off frequency (cut off at 25 Hz).  

I use the filtering phase as proposed at https://github.com/mrava87/Devito-fwi/blob/main/notebooks/acoustic/AcousticVel_L2_Nstages.ipynb.  

[further development is ongoing to make my research even better]
